<template>
    <HeaderMenu :isDarkMode="isDarkMode" @getViewMode="getViewMode" />
    <div v-if="displayCardName.length === 0">
        <div class="controls_menu" v-bind:style="isDarkMode ? 'background-color: #202d36;' : 'undefined;'">
            <div class="controls_menu_block1">
                <SearchField :displayCountries="options" :searchedText="searchedText" @search="getSearchData"
                    :isDarkMode="isDarkMode" />
            </div>
            <div class="controls_menu_block2">
                <RegionSelector :regions="regions" :searchedRegion="searchedRegion" @region="getSelectedRegion"
                    :isDarkMode="isDarkMode" />
            </div>
        </div>
        <div class="grid" v-bind:style="isDarkMode ? 'background-color: #202d36;' : 'undefined;'">
            <div v-if="displayCountries && displayCountries.length > 0" v-for="item in displayCountries" :key="item.area">
                <a @click="getClickedCard(item.name.common)">
                    <ItemCards :country="item" :isDarkMode="isDarkMode" />
                </a>
            </div>
            <div v-else class="label">
                <a-spin size="large" />
                <div v-if="errorMessage.length > 0">
                    {{ errorMessage }}
                </div>
            </div>
        </div>
    </div>
    <div v-else>
        <DisplayCardPage @setMainPage="setMainPage" :displayCountries="displayCountries" :displayCardName="displayCardName"
            :isDarkMode="isDarkMode" />
    </div>
</template>

<script>
import axios from 'axios';
import SearchField from './Controls/SearchField.vue'
import RegionSelector from './Controls/RegionSelector.vue'
import DisplayCardPage from './DisplayCardPage/DisplayCardPage.vue'
import ItemCards from './ItemCards/ItemCards.vue'
import HeaderMenu from './HeaderMenu/HeaderMenu.vue'
import ControlsMenu from './Controls/ControlsMenu/ControlsMenu.vue'

export default {
    components: {
        ItemCards,
        DisplayCardPage,
        HeaderMenu,
        ControlsMenu,
        SearchField,
        RegionSelector
    },
    name: 'App',
    data() {
        return {
            countries: [],
            errorMessage: '',
            regions: [],
            searchedText: '',
            searchedRegion: '',
            options: [],
            displayCountries: [],
            displayCardName: '',
            isDarkMode: false,
        }
    },
    watch: {

        regions() {
            this.getOptions(this.countries, this.regions, this.searchedText)
        },
        countries(newValue) {
            if (newValue) {
                this.getRegions(this.countries)
            }

        },
        searchedText() {
            this.getDisplayCountries(this.countries, this.searchedText, this.searchedRegion)
            this.getOptions(this.countries, this.regions, this.searchedText)
        },
        searchedRegion() {
            this.getDisplayCountries(this.countries, this.searchedText, this.searchedRegion)
        },
    },
    methods: {
        getViewMode() {
            if (this.isDarkMode) {
                this.isDarkMode = false
            } else {
                this.isDarkMode = true
            }
        },
        getClickedCard(selectedContry) {
            this.displayCardName = selectedContry
        },
        getDisplayCountries(countries, searchedText, searchedRegion) {
            this.displayCountries = countries.reduce((acc, country) => {
                if (country.name.common.toUpperCase().includes(searchedText.toUpperCase())
                    && country.region.toUpperCase().includes(searchedRegion.toUpperCase())) acc.push(country)
                return acc
            }, [])
        },
        getOptions(countries, regions, searchedText) {
            if (countries?.length > 0 && regions?.length > 0) {
                this.options = regions.reduce((acc, region) => {
                    acc.push(
                        {
                            value: region,
                            options: countries.reduce((acc, country) => {
                                if (country.region === region
                                    && country.name.common.toUpperCase().includes(searchedText.toUpperCase())) acc.push(
                                        {
                                            value: country.name.common
                                        }
                                    )
                                return acc
                            }, [])
                        }
                    )

                    return acc
                }, [])
            }
        },
        getRegions(countries) {
            this.regions = countries.reduce((acc, country) => {
                if (acc.indexOf(country.region) == -1) {
                    acc.push(country.region)
                }
                return acc
            }, [])
        },
        getSearchData(searchedText) {
            this.searchedText = searchedText
        },
        getSelectedRegion(region) {
            this.searchedRegion = region
        },
        setMainPage() {
            this.displayCardName = ''
        },
        async fetch() {
            try {
                await axios.get('https://restcountries.com/v3.1/all')
                    .then((response) => {
                        console.log("response.data", response.data)
                        this.countries = response.data
                        this.displayCountries = response.data
                    })
            } catch (e) {
                this.errorMessage = e.message
                console.error(e.message)
            }

        }
    },
    mounted() {
        this.fetch();
    }

}
</script>

<style lang="scss" scoped>
@import "./AppStyle.scss"
</style>