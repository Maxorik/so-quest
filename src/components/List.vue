<template>
  <div>
    <div v-for='parent in elements' :key='parent.id'>

      <div class='checkbox-block'>
        <input class="checkbox-square" 
              :id='parent.label_id' 
              type="checkbox" 
              :checked='parent.checked' 
              @click='parent_check(parent)' 
              :class="{ notfull: parent.isfull }" 
        >
        <label :for='parent.label_id'>{{parent.name}}</label>
      </div>

      <hr class='app_hr' />
      
      <List_item
        v-for='child of parent.childes' :key='child.id'
        v-bind:data='child'
        v-on:child_check='check_child'
      >

        <hr class='app_hr' /> 

      </List_item>

    </div>
  </div>
</template>

<script>
import List_item from './List_item.vue';
export default {
  name: 'List',
  props: {
    elements:{
      type:Array
    }
  }, 
  components: { List_item },
  methods:{
  /*клик на родителя*/
    parent_check(elem){
      elem.isfull = false;
      if(elem.checked == true){
      elem.checked = false;
        elem.checked_child = 0;
        for(let i=0; i<elem.childes.length; i++){
          elem.childes[i].checked = false;
        }
      }
      else{
        elem.checked = true;
        elem.checked_child = elem.childes.length;
        for(let i=0; i<elem.childes.length; i++){
          elem.childes[i].checked = true;
        }
      }
    },

    /*клик на потомка*/
    check_child(elem){

    /*определяем родителя потомка*/
      let my_parent = this.elements.find(item => item.id === elem.parent_id); 
      if(elem.checked == true){
        elem.checked = false;
        my_parent.checked_child--;
        if(my_parent.checked_child==0){
          my_parent.checked = false;
        }
      }
      else{
        elem.checked = true;
        my_parent.checked_child++;
        my_parent.checked = true;
      }

      /*если у родителя есть отмеченные потомки, но не все - родитель получает класс notfull - квадратик в чекбоксе*/
      my_parent.checked_child !=0 && my_parent.checked_child < my_parent.childes.length ? my_parent.isfull = true : my_parent.isfull = false;
    }
  },

  created(){

  /*фейковые данные с ресурса https://app.fakejson.com*/
    let data = JSON.stringify({ 
      "token": "4yN5zerNisvFGRvHaAy40g",      
      /*при многократном использовании (за день) токен перестает работать.. попробуйте IhbAeCuG4R0pzfs6ChBc1g или gXwndHrnCkDiGuYa_zqFLA
        Либо, как очень крайний вариант - необходимо будет зарегистрироваться на https://app.fakejson.com и получить новый токен, затем вставить его сюда
      */
      "data": {
        "id": "numberInt",
        "parent_id": "numberInt|1,3",  
        "name": "stringShort|1,1",
        "checked": "numberBool",
        "label_id": "stringCharacters", 
        "_repeat": 10
      }
    });

    let elements = this.elements; 

    let request = new XMLHttpRequest();

    /*запрос к ресурсу с данными */
    request.onload = function(){
      let fake_data = JSON.parse(this.response);
      if (request.status >= 200 && request.status < 400){

      /*Если данные пришли успешно, распределяем их по родителям, также изменяем параметры родителей*/
        for(let i=0; i<fake_data.length; i++){
          for(let j=0; j<elements.length; j++){
            if(fake_data[i].parent_id == elements[j].id){
              elements[j].childes.push(fake_data[i]);
              if(fake_data[i].checked == true){
                elements[j].checked_child++;
                elements[j].checked = true;
              }
            }
            elements[j].checked_child !=0 && elements[j].checked_child < elements[j].childes.length ? elements[j].isfull = true : elements[j].isfull = false;
          }
        }
      }
    }

    request.open("POST", "https://app.fakejson.com/q");
    request.setRequestHeader("content-type", "application/json");
    request.send(data);    
  }
}
</script>

<style lang="scss">
  @import '../scss/checkbox.scss'; 

  .checkbox-block{
    margin: 15px 10px;
  }

  .checkbox-child{
    margin-left:40px;
  }

  .app_hr{
    color:$hr-color;
    margin: 0 10px;
  }

</style>
