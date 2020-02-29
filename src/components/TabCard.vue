<template>
    <div class="card">
        <header class="card-header">
            <ul class="tab-heads">
                <li
                  class="tab-head"
                  v-for="tab in tabs"
                  :key="tab"
                  v-bind:class="{
                      'tab-head--active': activeTab === tab
                  }"
                  @click="switchTab(tab);"
                >
                    <slot :name="tabHeadSlotName(tab)">{{ tab }}</slot>
                </li>
            </ul>
        </header>
        <main class="card-body">
            <div class="tab-panel">
                <slot :name="tabPanelSlotName"></slot>
            </div>
        </main>
    </div>
</template>

<script>
    export default {
        props: {
            initialTab: String,
            tabs: Array
        },
        data () {
            return {
                activeTab: this.initialTab
            };
        },
        computed: {
            tabPanelSlotName() {
                return `tab-panel-${this.activeTab}`;
            }
        },
        methods: {
            tabHeadSlotName(tabName) {
                return `tab-head-${tabName}`;
            },
            switchTab(tabName) {
                this.activeTab = tabName;
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>