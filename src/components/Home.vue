<template>
  <div class="container">
      <h1 class="text-center mt-5">Vue Todo List</h1>
      <div class="row justify-content-center align-item-center mt-5">
        <div class="col-md-6">
          <div class="d-flex">
            <input type="text" v-model="description" placeholder="Enter a Todo List" class="form-control" />
            <button @click="addTodo" class="btn btn-success rounded-0 btn-lg">Add</button>
          </div>
        </div>
      </div>
      <div class="row justify-content-center align-item-center mt-5">
        <div class="col-md-6">
          <table class="table table-success table-hover">
            <thead>
              <tr>
                <th scope="col">id</th>
                <th scope="col">Task</th>
                <th></th>
                <th scope="col">Edit</th>
                <th scope="col">Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(todolist, index) in todolists" :key="index"  class="table-light">
                <th scope="row">{{index + 1}}</th>
                <td>{{todolist.description}}</td>
                <td>
                  <div v-if="select.id == index" class="d-flex text-center flex-column flex-lg-row">
                    <input v-model="editedDescription" type="text" class="form-control" />
                    <button @click="updateTodo(todolist, index)" class="btn"><span class="fa fa-floppy-disk"></span></button>
                  </div>
                </td>
                <td>
                  <div class="d-flex text-center">
                    
                    <button @click="editTodo(index,todolist)" class="btn"><span class="fa fa-pen-to-square h5"></span></button>                  
                  </div>
                </td>
                <td>
                  <div class="d-flex text-center">
                    <button @click="removeTodo(todolist, index)" class="btn"><span class="fa fa-trash h5"></span></button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
      
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Home',

  data(){
    return{
      todolists:[],
      description: "",
      editedDescription: "",
      select: {
        id: null,
        value: false
      }
    }
  },
  async mounted() {
    const response = await axios.get('api/todolist/')
    this.todolists = response.data
  },
  methods:{
    async addTodo() {
      if(this.description.length > 0){
        const response = await axios.post('api/todolist/create', { 
        description: this.description
        })
        this.todolists.push(response.data)
        this.description = ""
      }else{
        console.log("error: Enter a text")
      }
    },
    async removeTodo(todolist, index){
      await axios.delete('api/todolist/' + todolist.todo_id)
      this.todolists.splice(index, 1)
    },
    async editTodo(index, todolist){
      this.select = { id: index, value: true}
      this.editedDescription = todolist.description
    },
    async updateTodo(todolist, index){
      const response = await axios.put('api/todolist/' + todolist.todo_id,{
        description: this.editedDescription
      })
      this.todolists[index] = response.data
      this.select= false
      this.editedDescription= ""
    } 
  }
  
}
</script>

<style>

</style>