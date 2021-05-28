<template>
  <div>
    <h1>TODO - LIST</h1>
    <ul>
      <li v-for="item in todos" :key="item.id">{{item.deskripsi}}<button @click ="Hapus(item.id)">Hapus</button></li> 
    </ul>
    <input v-model="myText" type="text"/>
    <button @click="Tambah">Menambahkan</button>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    data: () => {
      return {
        todos: [],
        myText:''
      }
    },
    created: function () {
      axios.get('http://localhost:3000/todo')
      .then(result=>{
        this.todos = result.data   
      })
    },
    methods: {
      Tambah: function (){
        const addItem = {deskripsi: this.myText}
        axios.post('http://localhost:3000/todo', addItem)
          .then(()=>{
            this.todos.push(addItem)
            location.reload()
          })
        this.myText = ''
      },
      Hapus: function (id) {
        axios.delete(`http://localhost:3000/todo/${id}`)
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

