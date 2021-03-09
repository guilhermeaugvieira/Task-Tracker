<template>
  <div class="container">
    <Header />
    <AddTask />
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
      tasks: Object as () => IReminder[]
    };
  },
  created() {
    this.tasks = [
      {
        id: 1,
        text: "Doctors Appointment",
        day: "March 1st at 2:30 pm",
        reminder: true
      },
      {
        id: 2,
        text: "Meeting at School",
        day: "March 3rd at 1:30 pm",
        reminder: true
      },
      {
        id: 3,
        text: "Food Shopping",
        day: "March 3rd at 11:00 pm",
        reminder: false
      }
    ];
  },
  methods: {
    deleteTask(id: number) {
      this.tasks = this.tasks.filter((task: IReminder) => task.id !== id);
    },
    toggleReminder(id: number) {
      this.tasks = this.tasks.map((task: IReminder) =>
        task.id === id ? { ...task, reminder: !task.reminder } : task
      );
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
