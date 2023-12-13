<script setup>
import {ref, onMounted } from 'vue'
import axios from 'axios'
const task = ref([])
const taks_input = ref('')
const addTask = async () => {
        alert(totalTasks.value)
        const v = { "id": (Number(totalTasks) + 1), "task":taks_input.value, "status":"Ongoing", "favorite":0 }
        const res = await axios.post('http://127.0.0.1:3000/task/', v,
          {
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            }
          }
        );
        console.log(res);
    }
const getData = async () => {
  const res = await axios.get(
    'http://127.0.0.1:3000/task'
    )
    task.value = res.data
    console.log(res.data)
}
onMounted(getData)
</script>
<style>
    @import "bootstrap/dist/css/bootstrap.css";
    @import "bootstrap-icons/font/bootstrap-icons.css";
</style>

<template>
  <main>
    <div class="center" style="width: 100%;">
    <div class="center" style="display: flex;">
        <input type="text" id="txtaddTask"  class="form-control" v-model="taks_input" 
        placeholder="Add task"
        style="width: 180px;" />
        <Button
            @btn-click="addTask"
            :text="'Add Task'"
            :className="'btn btn-primary'"   
          />
    </div>
    </div>
    <div v-for="t in task"
      style="padding: 8px; width: 80%; border: 1px solid gray;">
      <table>
        <tr>
          <td>
             {{ t.id }}
          </td>
          <td>
             {{ t.task }}
          </td>
          <td>
            <i v-show="t.status === 'Completed'" 
            class="bi bi-check-circle-fill" 
            style="font-size: 24px; color: #EC7063;">
            </i>  
          </td>
          <td>
            <button v-show="t.status === 'Ongoing'" @click="completed(t.id)">Mark as completed</button>
          </td>
        </tr>
      </table>
    </div>
  </main>
</template>
<style scoped>
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