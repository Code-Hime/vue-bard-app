<template>
  <div class="main">
    <h1 class="title">Bardic Words</h1>
    <div class="container">
      <button class="button is-primary" @click="inspireMe()">Inspire!</button>
      <p>Inspire your comrades or yourself with Bardic Inspiration!</p>
      {{ inspo.description }} {{ fullSource }}
      <button class="button is-danger" @click="mockMe()">Mock!</button>
      <p>Bring your enemies down a size with Vicious Mockery!</p>
      {{ insult.description }}
    </div>
  <div>
  </div>
  </div>
</template>

<script>
import { lifecycleHooks } from "../shared/logging-mixin";
import { logger } from "../shared/logger";
import { myInspos } from "../shared/inspos";
import { myInsults } from "../shared/mockeries";

export default {
  name: "Main",
  data() {
    return {
      insults: [],
      inspos: [],
      randomNum: 0,
      insult: 'no insult loaded',
      inspo: 'no inspiring quote loaded'
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
      this.randomNum = Math.floor(Math.random() * this.inspos.length);
      this.inspo = this.inspos[this.randomNum];
    },
    async mockMe() {
      this.randomNum = Math.floor(Math.random() * this.insults.length);
      this.insult = this.insults[this.randomNum];
    }
  }
};
</script>
