<template>
    <div class="header_menu" v-bind:style="isDarkMode ? 'background-color: #2b3743;' : 'undefined;'">
        <div class="header_menu_block1" v-bind:style="isDarkMode ? 'color: white' : 'undefined;'">
            Where in the world?
        </div>
        <div class="header_menu_block2">
            <a-button @click="test" style="height: 40px; border-color: none; font-weight: 600;">
                <template #icon>
                    <star-outlined />
                </template>
                Dark Mode
            </a-button>
        </div>
    </div>
</template>

<script>
import { StarOutlined } from '@ant-design/icons-vue';

export default {
    props: {
        isDarkMode: Boolean
    },
    data() {
        return {
            mode: false
        }
    },
    components: {
        StarOutlined
    },
    methods: {
        test(event) {
            this.$emit("getViewMode", event)
        }
    },


}
</script>

<style lang="scss" scoped>
@import "./HeaderMenuStyle.scss"
</style>