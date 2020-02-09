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

<script lang='ts'>
import { Vue, Component, Prop } from 'vue-property-decorator'

@Component
export default class List_item extends Vue {
  @Prop(Object) readonly data: object | undefined
}

</script>

<style lang="scss">
  
</style>
