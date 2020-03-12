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
            <div class="inspo-display" v-if="inspoDisplay == true">
              <p class="tab-desc-inspo">{{ inspo.description }}</p>
              <p class="source">{{ fullSource }}</p>
            </div>
          </div>
          <div class="options">
            <a class="link" @click="resolveOptions()">Options</a>
          </div>
          <div class="options-display" v-if="options == true">
            <div class="row">
                <div class="column">
                <label class="container">
                  <input type="checkbox" for="opt-self" id="self" value="Self" v-model="checkedTarget" />
                  <span class="checkmark"></span>Inspire Self
                </label>
                </div>
              <div class="column">
                <label class="container">
                  <input type="checkbox" for="opt-other" id="others" value="Others" v-model="checkedTarget" />
                  <span class="checkmark"></span>Inspire Others
                </label>
              </div>
            </div>
            <label class="container">
              <input type="checkbox" for="opt-humor" id="humor" value="Humorous" v-model="checkedFilter" />
              <span class="checkmark"></span>Humorous Inspiration
            </label>
            <label class="container">
              <input type="checkbox" for="opt-bold" id="bold" value="Bold" v-model="checkedFilter" />
              <span class="checkmark"></span>Bold Inspiration
            </label>
            <label class="container">
              <input type="checkbox" for="opt-heart" id="heart" value="Heartening" v-model="checkedFilter" />
              <span class="checkmark"></span>Heartening Inspiration
            </label>
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
      inspoDisplay: false,
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
    },
    recipientFilter() {
      var targetFilter = [];
      //create target filter
      if(this.checkedTarget.length > 0)
      {
        this.checkedTarget.forEach(function(rec) {
          this.targetFilter.push({
            recipient: rec
          });
        })
      }
      return targetFilter;
    },
    typeFilter() {
      var typeFilter = [];
 
      //create type filter
      if (this.checkedFilter.length > 0) 
      {
        this.checkedFilter.forEach(function(type) {
          this.typeFilter.push({
            type: type
          });
        })
      }
      return typeFilter;
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
      this.defaultDisplay = false;
      this.inspoDisplay = true;
      this.options = false;

      if(this.checkedTarget.length > 0 && this.checkedFilter.length > 0)
      {
        this.buildFilterArray();
        this.randomNum = Math.floor(Math.random() * this.filteredInspos.length);
        this.inspo = this.filteredInspos[this.randomNum];
      }
      else
      {
        this.randomNum = Math.floor(Math.random() * this.inspos.length);
        this.inspo = this.inspos[this.randomNum];
      }
    },
    async mockMe() {
      this.defaultDisplay = false;
      this.randomNum = Math.floor(Math.random() * this.insults.length);
      this.insult = this.insults[this.randomNum];
    },
    clearAll() {
      this.defaultDisplay = false;
      this.inspoDisplay = false;
    },
    resolveOptions() {
      this.clearAll();

      if (this.options == true) {
        this.options = false;
        this.defaultDisplay = true;
      }
      else {
        this.options = true;
        this.defaultDisplay = false;
      }
    },
    buildFilterArray() {
      this.filteredInspos = [];

      var targetFilter = this.recipientFilter();
      var typeFilter = this.typeFilter();

      //create new array from full list filtering above two
      if(this.targetFilter.length > 0 && this.typeFilter.length > 0)
      {
        this.filteredInspos = this.inspos.filter(({type, recipient}) => targetFilter.some(x => x.recipient === recipient) && typeFilter.some(y => y.type === type));
      }
    }
  }
};
</script>
