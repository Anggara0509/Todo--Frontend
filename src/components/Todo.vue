<template>
 
 <div>
  <h1>To Do</h1>
  <ul>
    <li v-for="item in todos" :key="item.id">{{ item.deskripsi }}<button @click="hapus(item.id)">X</button></li>
  </ul>
  <input v-model="myText"/>
  <button @click="tambah">Add</button>
 </div>
</template>

<script>
import axios from 'axios'
export default {
  data:function() {
    return {
      todos: [],
      myText: ''
    }
  },
  created: function () {
    const username = localStorage.getItem('usr')
    const password = localStorage.getItem('pwd')
    axios.get('http://localhost:3000/todo', { headers: {username, password}})
    .then(result=>{
      this.todos = result.data   
    })
  },
  methods: {
    tambah: function (){
      const newItem = {deskripsi: this.myText}
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.post('http://localhost:3000/todo', newItem, { headers: {username, password}})
        .then((response)=>{
          this.todos.push({id:response.data.id, deskripsi: this.myText})
          //location.reload()
        })
    },
    hapus: function (id) {
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.delete(`http://localhost:3000/todo/${id}`, { headers: {username, password}})
        .then(()=>{
          for(var i = 0; i<this.todos.length; i++){
            if(this.todos[i].id == id) {
              this.todos.splice(i,1)
            }
          }
          
        })
    }
  }
}
</script>
