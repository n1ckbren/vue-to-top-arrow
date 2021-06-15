<script>
import Vue from 'vue';
import VueToTopArrow from '@/vue-to-top-arrow.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueToTopArrow
  }
});
</script>

<template>
  <div id="app">
    <vue-to-top-arrow />
  </div>
</template>

<style>
  body {
    margin: 0;
    height: 10000px;
    width: 100%;
    background-color: lightcyan;
  }
</style>