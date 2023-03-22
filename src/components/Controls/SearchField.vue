<template>
    <div class="certain-category-search-wrapper" style="width: 250px">
        <a-auto-complete v-model:value="search" class="certain-category-search"
            dropdown-class-name="certain-category-search-dropdown" :dropdown-match-select-width="500" style="width: 250px"
            :options="displayCountries">
            <a-input-search placeholder="Search for country..." size="large"></a-input-search>
            <template #option="item" style="background-color: red;">
                <template v-if="item.options">
                    <span>
                        {{ item.value }}
                    </span>
                </template>
                <template v-else>
                    <div style="display: flex; justify-content: space-between">
                        {{ item.value }}
                    </div>
                </template>
            </template>

        </a-auto-complete>
    </div>
</template>
<script>
import { ref } from 'vue';
import { UserOutlined } from '@ant-design/icons-vue';

export default {
    props: {
        displayCountries: String,
        searchedText: String,
        isDarkMode: Boolean,
    },
    components: {
        UserOutlined,
    },
    data() {
        return {
            search: ref(this.searchedText),
        };
    },
    watch: {
        search() {
            this.$emit('search', this.search)
        }
    },
};
</script>
<style lang="scss" scoped>
@import "./SearchFieldStyle.scss"
</style>
  