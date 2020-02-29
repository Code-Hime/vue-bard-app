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
          <p class="default-desc" v-if="defaultDisplay == true">Inspire your comrades or yourself with Bardic Inspiration!</p>
          <div v-else>
            <p class="tab-desc-inspo">{{ inspo.description }}</p>
            <p class="source">{{ fullSource }}</p>
          </div>
          <div>
            <a class="link" @click="inspireMe()">Inspire!</a>
          </div>
        </template>

        <template slot="tab-head-insult">
          <div @click="defaultDisplay = true">Insult</div>
        </template>

        <template slot="tab-panel-insult">
          <p class="default-desc" v-if="defaultDisplay == true">Bring your enemies down a size with Vicious Mockery!</p>
          <p class="tab-desc-insult" v-else><b>{{ insult.description }}</b></p>
          <a class="link" @click="mockMe()">Mock!</a>
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
      initialTab: 'inspo',
      tabs: ['inspo', 'insult']
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
  }
};
</script>
