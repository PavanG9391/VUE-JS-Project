<script setup>
import {ref,onMounted} from 'vue';

const name= ref("Pawan!!!")
const status = ref(true);
const tasks = ref(['Task One','Task Two','Task Three'])
const link = ref('https://www.google.com')
const newTask = ref('')

const addTask =()=>{
  if(newTask.value.trim() !==""){
    tasks.value.push(newTask.value)
    newTask.value=''
  }
}

const toggle =()=>{
  status.value = !status.value
}

const deleteTask=(index)=>{
  tasks.value.splice(index,1)
}

onMounted(async ()=>{
 try{
  const response = await fetch('https://jsonplaceholder.typicode.com/todos');
  const data = await response.json();
  tasks.value = data.map((task)=>task.title)
 } catch(error){
    console.log('Error fetching tasks')
 }
})
</script>

<template>
<h1>{{name}}</h1>
<p v-if="status">User is Active</p>
<p v-else>User is Inactive</p>

<h3>Tasks</h3>
<form v-on:submit.prevent="addTask">
  <input name="addTask" type="text" v-model="newTask" placeholder="add new task"/>
  <button type="submit">AddTask</button>
</form>
<ul>
  <li v-for="(task,index) in tasks" :key="index">
    <span>{{task}}</span> &nbsp;
    <button v-on:click="deleteTask(index)">X</button>
  </li>
</ul>

<a :href="link">Naviate to google</a>

<button v-on:click="toggle">Change Status</button>

</template>

