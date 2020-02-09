<template>
  <div id="app">
    <List />
  </div>
</template>

<script>
import List from './components/List.vue';
export default {
  name: 'App',
  components: { List }
}
</script>

<style lang="scss">
  body{
    margin:0;
    padding:0;
    background-color:#273339;
  }
</style>
