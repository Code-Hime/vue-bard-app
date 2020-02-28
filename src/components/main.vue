<template>
  <div class="main">
    <div class="content-container">
      <div class="section content-title-group">
        <h1 class="title">Bardic Words</h1>
      </div>

    </div>
  <div>
    <div class="card">
        <div class="card-content">
          <div class="content" v-if="selectedButton === 'inspire'">
            {{ inspo.description }} {{ fullSource }}
          </div>
          <div class="content" v-else-if="selectedButton === 'insult'">
            {{ insult.description }}
          </div>
          <div class="default-content" v-else>
            <h2>Click to:</h2>
            <p>Inspire your comrades or yourself with Bardic Inspiration!</p>
            <p>Bring your enemies down a size with Vicious Mockery!</p>
          </div>
        </div>
        <footer class="card-footer">
          <button class="button is-primary" @click="inspireMe()">Inspire!</button>
          <button class="button is-danger" @click="mockMe()">Mock!</button>
          <button class="button is-warning" @click="clear()">Clear</button>
        </footer>
      </div>
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
      inspo: 'no inspiring quote loaded',
      selectedButton: ''
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
      this.selectedButton = 'inspire';
      this.randomNum = Math.floor(Math.random() * this.inspos.length);
      this.inspo = this.inspos[this.randomNum];
    },
    async mockMe() {
      this.selectedButton = 'insult';
      this.randomNum = Math.floor(Math.random() * this.insults.length);
      this.insult = this.insults[this.randomNum];
    },
    clear() {
      this.selectedButton = '';
    }
  }
};
</script>
