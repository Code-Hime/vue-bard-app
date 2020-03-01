<template>
  <div class="main">
    <div class="content-container">
      <div class="section content-title-group">
        <h1 class="title">Bardic Works</h1>
      </div>
    </div>

    <div>
      <TabCard :tabs="tabs" :initialTab="initialTab">
        <template slot="tab-head-inspo">
          <div @click="defaultDisplay = true">Inspire</div>
        </template>

        <template slot="tab-panel-inspo">
          <div class="default-desc" v-if="defaultDisplay == true">
            <p>Inspire your comrades or yourself with Bardic Inspiration!</p>
          </div>
          <div v-else>
            <p class="tab-desc-inspo">{{ inspo.description }}</p>
            <p class="source">{{ fullSource }}</p>
          </div>
          <div class="options">
            <a class="link" @click="resolveOptions()">Options</a>
          </div>
          <div class="options-display" v-if="options == true">
            <ul>
              <li><input type="checkbox" for="opt-self" id="self" value="Self" v-model="checkedTarget" />Inspire Self</li>
              <li><input type="checkbox" for="opt-other" id="others" value="Others" v-model="checkedTarget" />Inspire Others</li>
            </ul>
            <ul>
              <li><input type="checkbox" for="opt-humor" id="humor" value="Humorous" v-model="checkedFilter" />Humorous Inspiration</li>
              <li><input type="checkbox" for="opt-bold" id="bold" value="Bold" v-model="checkedFilter" />Bold Inspiration</li>
              <li><input type="checkbox" for="opt-heart" id="heart" value="Heartening" v-model="checkedFilter" />Heartening Inspiration</li>
            </ul>
          </div>
          <div class="inspire-button">
            <a class="link" @click="inspireMe()">Click to Inspire!</a>
          </div>
        </template>

        <template slot="tab-head-insult">
          <div @click="defaultDisplay = true">Insult</div>
        </template>

        <template slot="tab-panel-insult">
          <p class="default-desc" v-if="defaultDisplay == true">Bring your enemies down a size with Vicious Mockery!</p>
          <p class="tab-desc-insult" v-else><b>{{ insult.description }}</b></p>
          <div class="mock-button">
            <a class="link" @click="mockMe()">Click to Mock!</a>
          </div>
        </template>
      </TabCard>
    </div>

  </div>
</template>

<script>
import { lifecycleHooks } from "../shared/logging-mixin";
import { logger } from "../shared/logger";
import { myInspos } from "../shared/inspos";
import { myInsults } from "../shared/mockeries";
import TabCard from '@/components/TabCard';

export default {
  name: "Main",
  components: {
    TabCard
  },
  data() {
    
    return {
      insults: [],
      inspos: [],
      randomNum: 0,
      insult: 'no insult loaded',
      inspo: 'no inspiring quote loaded',
      defaultDisplay: true,
      options: false,
      initialTab: 'inspo',
      tabs: ['inspo', 'insult'],
      checkedTarget: [],
      checkedFilter: [],
      filteredInspos: []
    };
  },
  mixins: [lifecycleHooks],
  created() {
    this.loadInsults();
    this.loadInspos();
    logger.info(`${this.componentName} created hook called`);
  },
  computed: {
    fullSource() {
      if (this.inspo.source) {
        return `— ${this.inspo.source}`
      }
      else {
        return '';
      }
    }
  },
  methods: {
    async getInsults() {
      return new Promise(resolve => {
        resolve(myInsults);
      })
    },
    async loadInsults() {
      this.insults = [];
      this.insults = await this.getInsults();
    },
    async getInspos() {
      return new Promise(resolve => {
        resolve(myInspos);
      })
    },
    async loadInspos() {
      this.inspos = [];
      this.inspos = await this.getInspos();
    },
    async inspireMe() {
      this.clear();
      this.randomNum = Math.floor(Math.random() * this.inspos.length);
      this.inspo = this.inspos[this.randomNum];
    },
    async mockMe() {
      this.clear();
      this.randomNum = Math.floor(Math.random() * this.insults.length);
      this.insult = this.insults[this.randomNum];
    },
    clear() {
      this.defaultDisplay = false;
    },
    resolveOptions() {
      if (this.options == true) {
        this.options = false;
        this.defaultDisplay = true;
      }
      else {
        this.options = true;
        this.defaultDisplay = false;
      }
    },
    buildFilterArray(/*array, filter*/) {
      //create target filter
      
      //create type filter

      //create new array from full list filtering above two
      //this.filteredInspos = this.inspos.filter(inspos, function(inspo) { return inspo.target = target_filter && inspo.type = type_filter});
      
      //return new array to inspireMe()
    }
  }
};
</script>
