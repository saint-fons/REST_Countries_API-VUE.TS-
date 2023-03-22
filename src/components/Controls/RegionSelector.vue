<template>
    <div class="demo-dropdown-wrap">
        <a-dropdown trigger="['click']">
            <template #overlay>
                <a-menu @click="handleMenuClick">
                    <a-menu-item v-if="regions && regions.length > 0" v-for="region in regions" :key="`${region}`">
                        {{ region }}
                    </a-menu-item>
                </a-menu>
            </template>
            <a-button style="height: 40px;">
                {{ searchedRegion && searchedRegion.length > 0 ? searchedRegion : 'Filter by Region' }}
                <DownOutlined />
            </a-button>
        </a-dropdown>
    </div>
</template>
<script>
import { defineComponent } from 'vue';
import { UserOutlined, DownOutlined } from '@ant-design/icons-vue';
import { ref } from 'vue'
import { CloseOutlined } from '@ant-design/icons-vue';

export default defineComponent({
    components: {
        UserOutlined,
        DownOutlined,
        CloseOutlined
    },
    props: {
        regions: [],
        searchedRegion: ""
    },
    data() {
        return {
        }
    },
    setup() {
        const selectedRegion = ref('');
        const handleMenuClick = e => {
            selectedRegion.value = e.key !== selectedRegion.value ? e.key : "";
        };
        return {
            handleMenuClick,
            selectedRegion
        }
    },
    watch: {
        selectedRegion(newValue) {
            this.$emit('region', newValue)

        }
    },
    methods: {
        getRegion(selectedRegion) {
            this.selectedRegion = selectedRegion
        }
    }
});
</script>
<style lang="scss" scoped>
@import './RegionSelectorStyle.scss'
</style>
  