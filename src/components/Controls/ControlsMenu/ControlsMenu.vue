<template>
    <div>
        <div class="controls_menu">
            <div class="controls_menu_block1">
                <SearchField :displayCountries="displayCountries" :searchedText="searchedText" @search="getSearchData" />
            </div>
            <div class="controls_menu_block2">
                <RegionSelector :regions="regions" @region="getSelectedRegion" />
            </div>
        </div>
    </div>
</template>

<script>
import SearchField from '../SearchField.vue'
import RegionSelector from '../RegionSelector.vue'


export default {
    props: {
        displayCountries: [],
        searchedText: [],
        getSearchData: Function
    },
    components: {
        SearchField,
        RegionSelector
    },
    data() {
        return {
            searchedText1: ""
        }
    },
    methods: {
        getSearchData(searchedText) {
            this.searchedText1 = searchedText
        },
    }
}
</script>

<style lang="scss" scoped>
@import './ControlsMenuStyle.scss'
</style>