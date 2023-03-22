<template>
    <div class="container" v-bind:style="isDarkMode ? 'background-color: #202d36;' : 'undefined;'">
        <div class="controls">
            <a-button @click="setMainPage()">
                <template #icon>
                    <arrow-left-outlined />
                </template>
                Back
            </a-button>
        </div>
        <div class="container_Info" v-if="currentItem?.name?.common.length > 0"
            v-bind:style="isDarkMode ? 'color: white;' : 'undefined;'">
            <div class="container_Info_block1">
                <img :src="`${currentItem.flags.svg}`" class="item_image" alt="" @load="onImgLoad" @error="onImgError">
            </div>
            <div class="container_Info_block2">
                <div class="item_info">
                    <div class="title">

                        {{ currentItem.name.common }}
                    </div>
                    <div class="item_info_list">
                        <div class="info_list_block1">
                            <div>
                                <span style="font-weight: 600;">Native name: </span>{{
                                    currentItem.name.official }}
                            </div>
                            <div>
                                <span style="font-weight: 600;">Population: </span>{{
                                    currentItem.population }}
                            </div>
                            <div>
                                <span style="font-weight: 600;">Region: </span>{{
                                    currentItem.region }}
                            </div>
                            <div>
                                <span style="font-weight: 600;">Sub Region: </span>{{
                                    currentItem.subregion }}
                            </div>
                            <div>
                                <span style="font-weight: 600;">Capital: </span>{{
                                    currentItem.capital.join(', ') }}
                            </div>
                        </div>
                        <div class="info_list_block2">
                            <div>
                                <span style="font-weight: 600;">Top Level Domain: </span>{{
                                    currentItem.tld.join(', ') }}
                            </div>
                            <div>
                                <span style="font-weight: 600;">Currencies: </span>{{
                                    Object.values(this.currentItem.currencies).map(currency => currency.name).join(', ') }}
                            </div>
                            <div>
                                <span style="font-weight: 600;">Languages: </span>{{
                                    Object.values(this.currentItem.languages).join(', ') }}
                            </div>
                        </div>
                    </div>
                </div>
                <div class="linkItems" v-if="this.currentItem.borders?.length > 0"
                    v-bind:style="isDarkMode ? 'color: white;' : 'undefined;'">
                    <span style="font-weight: 600;">Border countries: </span>
                    <span class="linkItems_items" v-for="item in this.currentItem.borders">
                        <a-button>{{ item }}</a-button>
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { ArrowLeftOutlined } from '@ant-design/icons-vue';

export default {
    props: {
        displayCountries: [],
        displayCardName: '',
        currentItem: {},
        isDarkMode: Boolean,
    },
    data() {
        return {
            currentItem: {},
        }
    },
    components: {
        ArrowLeftOutlined
    },
    mounted() {
        this.getCurrentItem(this.displayCountries, this.displayCardName)
    },
    methods: {
        setMainPage() {
            this.$emit('setMainPage')
        },
        getCurrentItem(items, searchedText) {
            const currentItemIndex = items.findIndex(item => {
                return item.name.common === searchedText
            })
            this.currentItem = items[currentItemIndex]
        }
    },
}
</script>

<style lang="scss" scoped>
@import "./DisplayCardPageStyle.scss"
</style>