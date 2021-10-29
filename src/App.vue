<template>
  <div id="container">
    <div class="row bg-light">
       <div class="col-12 text-center">
          <h3 class="mt-5">Todo List | Vue.Js</h3>
          <hr>
          <div class="row">
            <div class="col-6 offset-3  d-flex justify-content-between align-items-center ">
              <input type="text" v-model="todoText">
              <button @click="addTodo()" class="btn btn-primary">Ekle</button>
            </div>
          </div>
          <hr>
          <div class="todo-container">
            <Todo @deleteTodo="deleteTodo($event)" v-for="todo in todoList" :todo="todo" :key="todo.id" />
          </div>
       </div>
    </div>
  </div>
</template>

<script>
import Todo from "@/components/Todo"
import axios from "axios"

export default {
  components : {
    Todo
  },
  data(){
    return{
      todoText : "",
      todoList : []
    }
  },
  methods : {
    addTodo(){
      axios.post('https://todo-app-vue1-default-rtdb.firebaseio.com/todoList.json',{text : this.todoText})
      .then(response => {
        this.todoList.push({
          id : response.data.name,
          text : this.todoText
        })
        this.todoText = ''
      })
      .catch(e => {
        console.log(e);
      })
      
    },
    deleteTodo(todoID){
      axios.delete("https://todo-app-vue1-default-rtdb.firebaseio.com/todoList/" + todoID + ".json")
      .then(response => {
        let index = this.todoList.findIndex( e => {
          return e.id == todoID
        })
        this.todoList.splice(index,1)
      })
      .catch(e => {
        console.log(e);
      })
    }
  },
   created(){
      axios.get('https://todo-app-vue1-default-rtdb.firebaseio.com/todoList.json')
      .then(response => {
        console.log(response.data);
        for(let key in response.data){
           
          let todo = {
            text : response.data[key].text,
            id : key
          }
          this.todoList.push(todo)
        }
      })
      .catch(e => {
        console.log(e);
      }
        
      )
    }
 
}
</script>

