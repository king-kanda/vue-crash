<script setup>
import { ref , onMounted} from 'vue'

  const name = ref('john doe')
  const status = ref('active')
  const tasks = ref(['Task One' , 'Task Two' , 'Task Three' ])
  const newTask = ref('New Task')

  const toggleStatus = () => {
    if(status.value === 'active'){
      status.value = 'pending'
    }else if (status.value === 'pending') {
      status.value = 'inactive';
    } else {
      status.value = 'active';
    }
  };

  const addTask = () => {
    console.log("fire wire");
    if(newTask.value.trim() !== '' ){
        tasks.value.push(newTask.value)
        newTask.value = ''
    }
  }

  const deleteTask = (index) =>{
     console.log("deleting tasks")
     tasks.value.splice(index , 1)

  }

  onMounted(async() =>{
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos')
        const data = await response.json()
        tasks.value = data.map((task) => task.title )
    } catch (error) {
        console.log("Error")
    }
  })
  
 
</script>

<template>
  <h1>
    Jambo , {{ name }}
  </h1>
  <p>
    this job is {{ status }}
  </p>

  <div class="form">
    <form action="" @submit.prevent="addTask" >
      <label for="newTask">Add task</label>
      <input type="text" name="newTask" id="newTask" v-model="newTask">
      <button type="submit">
        Add Task
      </button>
    </form>
  </div>

  <div class="div">
    Task are
    <ul>
      <li v-for="(task ,index ) in tasks" :key="task"  class="list">
          {{ task }}

          <button class="delete" @click="deleteTask(index)">
            Delete task
          </button>

      </li>
    </ul>
  </div>
  <br>
  <div >
    <button @click="toggleStatus" >
      Toggle Status
    </button>
  </div>
</template>

<style scoped >

h1{
  color:white;
  font-weight: bolder;
 
}
p{
  color:whitesmoke;
}

.div{
  padding :16px;
  height: 40vh;
  overflow-y: scroll;
}

.delete{
  background-color: red;
  padding: 8px ;
  color: white;
}

.list{
  margin: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 0;
}
</style>