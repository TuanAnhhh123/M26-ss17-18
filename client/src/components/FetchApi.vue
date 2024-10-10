<script setup>
import { onMounted, reactive, ref } from 'vue';
const users = ref([])
const isShowLoang = ref(false)
const loadData = async () => {
  // fetch("http://localhost:8080/user")
  // .then(respons=> respons.json())
  // .then(data=>console.log(data))
  // .catch((error)=> console.log(error))
try{
  isShowLoang.value = true;
  const response = await fetch("http://localhost:8080/user");

  const data = await response.json();
  users.value = data
  }catch (error){
    return error
  }finally{
    isShowLoang.value = false;
  }
};

  onMounted(async() => {
    loadData()
  }) 

  const userInfo = reactive({
    name: "",
    age: 0
  })
  const handleSubmit = async () => {
    const response =  await fetch("http://localhost:8080/user", {
      method: "POST",
      headers:{
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(userInfo),
    })
    loadData()
  }
  const handelDEl = async (id) =>{
    const response = await fetch(`http://localhost:8080/user/${id}`,{
      method: "DELETE"
    })
    loadData()
  }
</script>

<template>
  <div>
    <form @submit.prevent="handleSubmit"> 
      <div>
        <label for="">Name</label>
        <input v-model="userInfo.name" type="text">
      </div>
      <div>
        <label for="">Age</label>
        <input v-model="userInfo.age" type="number">
      </div>
      <button type="submit">Add</button>
    </form>
    <table border="1">
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Age</th>
          <th>option</th>
        </tr>
      </thead>
      <tbody  v-for="user in users" :key="user.id">
        <tr>
          <div v-if="isShowLoang">Loading.....</div>
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.age }}</td>
          <td><button @click="handelDEl(user.id)">Xóa</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
</style>
