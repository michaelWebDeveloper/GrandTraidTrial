<template>
  <div id="app">
    <NameSearch />
  </div>
</template>

<script>
import NameSearch from './components/NameSearch.vue';

export default {
  components: {
    NameSearch,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
  display: flex;
  justify-content: center;
}
</style>