<template>
  <v-card class="ma-9 pa-3" outlined elevation="2">
    <v-text-field
      v-model="newTaskTitle"
      class="pa-3"
      outlined 
      label="Add Task"
      append-icon="mdi-plus-box"
      hide-details
      @keyup.enter="addTask"
      @click:append="addTask"
    >
    </v-text-field>
    <v-list
      flat
      subheader
    >
      <v-list-item v-for="task in tasks" :key="task.id" @click="doneTask(task.id)" 
        :class="{ 'blue lighten-5 ' : task.done }"
      >
        <template v-slot:default>
          <v-container>
            <v-row align="center">
              <v-col class="d-flex" cols="12" md="9">
                <v-list-item-action>
                  <v-checkbox :input-value="task.done"></v-checkbox>
                </v-list-item-action>

                <v-list-item-content>
                  <v-list-item-title :class="{ 'text-decoration-line-through' : task.done }">{{ task.title }}</v-list-item-title>
                </v-list-item-content>
              </v-col>
              <v-col class="d-flex" cols="10" md="2">
              <!-- v-model does to two way binding to stuff in data (here the task.priority) -->
              <v-select
                v-model="task.priority" 
                :background-color="task.priorityColor"
                hide-details
                :items="priorityList"
                label="Set priority"
                @click.stop
                @change="setPriority(task.id)"
                solo
              ></v-select>
              </v-col>
              <v-col class="d-flex" cols="2" md="1">
              <v-list-item-action>
              <v-btn icon @click.stop="deleteTask(task.id)" class="taskDeleteBtn"> 
                <v-icon color="lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
            </v-col>
          </v-row>
        </v-container>
        </template>
      </v-list-item>      
    </v-list>
  </v-card>
</template>

<script>
  export default {
    name: 'Home',
    data() {
      return {
        newTaskTitle: '',
        tasks: [
          { id:1, title: 'Do groceries today at 6' ,done:false, priority: '', priorityColor: 'white', priorityIndex: 4},
          { id:2, title: 'Help Alex with his project' ,done:false, priority: '', priorityColor: 'white', priorityIndex: 4},
          { id:3, title: 'Get the Monitor from ES office' ,done:false, priority: '', priorityColor: 'white', priorityIndex: 4}
        ],
        priorityList: [ 'Low Priority', 'Medium Priority', 'High Priority']    
      }
    },
    methods: {
      doneTask(id) {
        let task = this.tasks.filter(task => task.id === id)[0]
        task.done = !task.done
      },
      deleteTask(id) {
        this.tasks = this.tasks.filter(task => task.id !== id)
      },
      addTask() {
        let newTask = {
          id: Date.now(),
          title: this.newTaskTitle,
          done: false,
          priority: ''
        }
        this.tasks.unshift(newTask)
        this.newTaskTitle = ''
      },
      setPriority(id) {
        let task = this.tasks.filter(task => task.id === id)[0]        
        if(task.priority === "Low Priority") {
          task.priorityIndex = 3
          task.priorityColor = "blue"
        } else if(task.priority === "Medium Priority") {
          task.priorityColor = "yellow"
          task.priorityIndex = 2
        } else {
          task.priorityColor = "red"
          task.priorityIndex = 1
        }
        this.tasks = this.tasks.sort((a, b) => (a.priorityIndex > b.priorityIndex ? 1 : -1))
        console.log(task.priority)
        console.log(this.tasks)
      }
    }
  }
</script>

<style scoped>
.taskDeleteBtn:hover {
  color:red;
}
</style>
