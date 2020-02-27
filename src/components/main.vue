<template>
  <div class="main">
    <h1 class="title">Bardic Words</h1>
    <div class="container">
      <button class="button is-primary">Inspire!</button>
      <p>Inspire your comrades or yourself with Bardic Inspiration!</p>
      <button class="button is-danger">Mock!</button>
      <p>Bring your enemies down a size with Vicious Mockery!</p>
    </div>
    <ul>
      <li v-for="insult in insults" :key="insult.id">
        {{ insult.description }}
      </li>
      <li v-for="inspire in inspires" :key="inspire.id">
        {{ inspire.description }}
      </li>
    </ul>
  </div>
</template>

<script>
import inspos from "../shared/inspos.json";
import mockeries from "../shared/mockeries.json";

export default {
  name: "Main",
  data() {
    return {
      insults: mockeries,
      inspires: inspos
    };
  }
};
</script>
