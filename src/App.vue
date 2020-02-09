<template>
  <div id="app">
    <List
      v-bind:elements='elements'
    />
  </div>
</template>

<script>
import List from './components/List.vue';
export default {
  name: 'App',
  components: { List },
  data(){
    return{
    /*массив родительских элементов*/
      elements:[    
      {
        id:1,
        checked_child:0,    
        name:'Parent 1',
        label_id: 'parent_1',
        checked: false,
        isfull:false,
        childes: []          
      },
      {
        id:2,
        checked_child:0,
        name:'Parent 2',
        label_id: 'parent_2',
        checked: false,
        isfull:false,
        childes: []
      },
      {
        id:3,
        checked_child:0,
        name:'Parent 3',
        label_id: 'parent_3',
        checked: false,
        isfull:false,
        childes: []
      },
      {
        id:4,
        checked_child:0,
        name:'Parent 4',
        label_id: 'parent_4',
        checked: false,
        isfull:false,
        childes: []
      }
    ]
    }
  }
}
</script>

<style lang="scss">
  body{
    margin:0;
    padding:0;
    background-color:#273339;
  }
</style>
