<template>
    <div class="country" v-bind:style="isDarkMode ? 'background-color: #2b3743;' : 'undefined;'">
        <img :src="`${country.flags.svg}`" class="item_image" alt="" @load="onImgLoad" @error="onImgError">
        <div class="container" v-bind:style="isDarkMode ? 'color: white;' : 'undefined;'">
            <div class="title">
                {{ country.name.common }}
            </div>
            <div>
                <span>Population: </span> {{ country.population }}
            </div>
            <div>
                <span>Region: </span> {{ country.region }}

            </div>
            <p v-if="country.capital?.length > 0">
                <span>Capital: </span> {{ country.capital?.join(', ') }}
            </p>
        </div>
    </div>
</template>

<script>

export default {
    components: {
    },
    props: {
        country: {},
        isDarkMode: Boolean
    },
    data() {
        return {
            isLoaded: false,
            isImageLoaded: false
        }
    },
    methods: {
        onImgLoad() {
            this.isLoaded = true
        }
    },
    watch: {
        isLoaded() {
            this.isImageLoaded = true
        }
    }
}
</script>

<style lang="scss" scoped>
@import "./ItemCardsStyles.scss"
</style>