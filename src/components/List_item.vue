<template>
 <div>
  <div class='checkbox-block checkbox-child'>
   <input class="checkbox-square" 
    :id='data.label_id' 
    type="checkbox" 
    :checked='data.checked' 
    @click='$emit("child_check", data)'
   >
   <label :for='data.label_id'>{{data.name}}</label>
  </div>
  <hr class='app_hr' />
 </div>
</template>

<script>
export default {
  name: 'List_item',
  props: {
   data:{
    type: Object
   }
  }
}

</script>

<style lang="scss">
  
</style>
