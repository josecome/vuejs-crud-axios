<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
const tasks  = ref([])
const task_input = ref('')
const getData = async () => {
  const res = await axios.get(
    'http://127.0.0.1:3000/task'
  )
  console.log(res.data)
  tasks.value = res.data
}
const completed = async (id) => {
  let confirm_message = "Confirm to update task as completed"
  if(confirm(confirm_message) == false) {
    return false;
  }
  const v = {"status": "Completed"}
  const res = await axios.patch(
    `http://127.0.0.1:3000/task/${ id }`, v,
    {
      headers: {
        'Accept': 'application/json',
        'Content-type': 'application/json'
      }
    }
  );
  console.log(res)
  refreshPage();
}
const addTask = async () => {
  const v = {"id": tasks.value.length + 1, 
  "task": task_input.value,
  "status": "Ongoing"}
  const res = await axios.post(
    `http://127.0.0.1:3000/task`, v,
    {
      headers: {
        'Accept': 'application/json',
        'Content-type': 'application/json'
      }
    }
  );
  console.log(res)
  refreshPage();
}
onMounted(getData)
const refreshPage = () => {
  location.reload()
}
</script>
<style>
    @import "bootstrap/dist/css/bootstrap.css";
    @import "bootstrap-icons/font/bootstrap-icons.css";
</style>
<template>
 <input type="text" v-model="task_input" 
 class="form-control"
 style="width: 200px; display: inline-block;"
 />  
 <button
 @click="addTask"
 class="btn btn-primary"
 style="display: inline-block;"
 >
  Add Task
 </button>
<div 
  v-for="task in tasks"  
  style="padding: 8px; width: 80%; border: 1px solid gray;"

>
<table>
  <tr>
    <td>
      <div id="iddiv">{{ task.id }}</div>
    </td>
    <td class="width160">
      {{ task.task }}
    </td>
    <td>
      <i v-show="task.status === 'Completed'"
      class="bi bi-check-circle-fill"
      style="font-size: 24px; color: red"
      ></i>
    </td>
    <button v-show="task.status === 'Ongoing'"
    @click="completed(task.id)"
    >
    Mark as completed
    </button>
  </tr>
</table>
</div>
</template>
<style scoped>
.width160 {
  width: 220px;
}
.center {
  margin: auto;
  width: 50%;
  padding: 10px;
}
#iddiv {
    width: 50px; 
    height: 50px; 
    text-align: center; 
    border-radius: 50%; 
    background-color: #2ECC71; 
    font-size: 28px; 
    color: white; 
    padding: 2;
}
</style>