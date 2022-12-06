<template>
  <div>
  <v-app class="indigo">
    <v-content style="bg-color:blue">
      <v-container>
        <v-layout row>
          <v-flex xs12>
            <v-header white>
              <v-card class="flex" flat title>
                <v-card-actions class=" justify-center">
                  <h2>List To Do</h2>
                </v-card-actions>
              </v-card>
            </v-header>
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex sx6>
            <v-from ref="form" class="white ma-5">
              <v-text-field class="pa-3" v-model="task" label="Task"></v-text-field>
              <v-text-field class="pa-3" v-model="description" label="Description"></v-text-field>
              <v-btn v-if="edit!=true " color="primary" @click="addTask(task,description,$event)">Add Task</v-btn>
              <v-btn v-if="edit==true" color="success" @click="updateTask($event)">Update</v-btn>
              <v-btn v-if="edit==true" color="secondary" @click="cancelTask($event)">Cancel</v-btn>
            </v-from>
          </v-flex>
          <v-flex xs12 style="padding-top:30px">
            <v-flex xs12 v-for="(task,index) in tasks" :key="index">
              <v-card class="mb-3">
              <v-card-title primary-title>
                <div>
                  <h3 class="headline mb-0">Task: {{task.task}}</h3>
                  <div>Description: {{task.description}}</div>
                </div>
              </v-card-title>
              <v-card-actions v-if="edit==false">
                <v-btn color="warning" @click="editTask(task,index)">Edit</v-btn>
                <v-btn color="error" @click="deleteTask(index)">Delete</v-btn>
              </v-card-actions>
              <v-card-actions v-if="edit!=false">
                <v-btn disable color="warning">Edit</v-btn>
                <v-btn disable color="error">Delete</v-btn>
              </v-card-actions>
              </v-card>
            </v-flex>
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex xs12>
            <v-footer dark>
              <v-card class="flex" flat title>
                <v-card-actions class=" justify-center">
                  <strong>Made by Thao</strong>
                </v-card-actions>
              </v-card>
            </v-footer>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</div>
</template>

<script>


export default {
  data: () => ({
    task:'',
    description:'',
    edit:false,
    delete:false,
    cancel:false,
    id2:0,
    index:0,
    tasks:[]
  }),
  methods:{
    addTask:function(t,d,e){
      e.preventDefault();
      this.tasks.push({
        task: this.task,
        description: this.description,
      });
      localStorage.setItem('tasks',JSON.stringify(this.tasks));
      this.task='';
      this.description='';
    },
    editTask:function(t,i){
      this.edit =! this.edit;
      this.task = t.task;
      this.description = t.description;
      this.index = i
    },
    updateTask:function(e){
      e.preventDefault();
      this.edit =! this.edit;
      let taskdb = {
        task: this.task,
        description: this.description,
      }
      this.tasks[this.index] = taskdb;
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
      let taskDB = JSON.parse(localStorage.getItem('tasks'))
      this.tasks= taskDB
      this.task='';
      this.description='';
   },
    cancelTask:function(e){
      e.preventDefault();
      this.task='';
      this.description='';
      this.edit =! this.editTask
    },
    deleteTask:function(i) {
      this.tasks.splice(i,1);
      localStorage.setItem('tasks',JSON.stringify(this.tasks))
    },
    
  },
  created() {
      let taskDB = JSON.parse(localStorage.getItem('tasks'))
      if (taskDB) {
        this.tasks = taskDB;
      }
    },
};
</script>
