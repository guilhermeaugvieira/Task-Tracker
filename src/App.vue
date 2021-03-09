<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" :showAddTask="showAddTask" />
    <div v-if="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Task
      @delete-task="deleteTask"
      @toggle-reminder="toggleReminder"
      v-for="task in tasks"
      :key="task.id"
      :task="task"
    />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Header from "./components/Header.vue";
import Task, { IReminder } from "./components/Task.vue";
import AddTask from "./components/AddTask.vue";

@Options({
  components: {
    Header,
    Task,
    AddTask
  },
  data() {
    return {
      tasks: Object as () => IReminder[],
      showAddTask: Boolean
    };
  },
  created() {
    this.fetchTasks().then((data: IReminder[])=> {
      this.tasks = data;
    }).catch((erroApi: Error)=> {
      console.log(erroApi)
    });
    this.showAddTask = false;
  },
  methods: {
    async deleteTask(id: number) {
      try {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
          headers: {
            'Content-type': 'application/json',
          },
        });

        this.tasks = this.tasks.filter((task: IReminder) => task.id !== id);

      } catch (errorApi) {
        console.log(errorApi);    
      }  
    },

    async toggleReminder(task : IReminder) {
      task.reminder = !task.reminder;

      try{

        const res = await fetch(`api/tasks/${task.id}`, {
          method: 'PUT',
          headers: {
            'Content-type': 'application/json',
          },
          body: JSON.stringify(task)
        });

        try {
          const data = await res.json();

          this.tasks = this.tasks.map((task: IReminder) =>
            task.id === task.id ? data : task
          );
        } catch (errorJson) {
          console.log(errorJson);
        }
      }catch(errorApi){
        console.log(errorApi);
      }
    },

    async addTask(task: IReminder) {
      try {
        const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
        
        });

        try {
          const data = await res.json();

          this.tasks.push(data);

        } catch (errorJson) {
          console.log(errorJson)
        }

      } catch (errorApi) {
        console.log(errorApi);
      }
    },

    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },

    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();

      return data;
    },

    async fetchTask(id: number) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();

      return data;
    }
  }
})
export default class App extends Vue {}
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;

  &:focus {
    outline: none;
  }

  &:active {
    transform: scale(0.98);
  }

  &-block {
    display: block;
    width: 100%;
  }
}
</style>
