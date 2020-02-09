<template>
  <div id="app">
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

        <div v-for='child in parent.childes' :key='child.id'>
          <div class='checkbox-block checkbox-child'>
            <input class="checkbox-square" 
              :id='child.label_id' 
              type="checkbox" 
              :checked='child.checked' 
              @click='child_check(child)'
            >
            <label :for='child.label_id'>{{child.name}}</label>
          </div>

          <hr class='app_hr' />

        </div>
      
    </div>
  </div>
</template>

<script>
//import List from './components/List.vue';
export default {
  name: 'App',
//components: { List },

  data(){
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
  },
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
    child_check(elem){

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
      "token": "6KXHC8cCVG0GA3KwvM3FNg",      
      /*при многократном использовании (за день) токен перестает работать.. попробуйте IhbAeCuG4R0pzfs6ChBc1g или gXwndHrnCkDiGuYa_zqFLA*/
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
  body{
    margin:0;
    padding:0;
    background-color:#273339;
  }
</style>
