<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Task
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder"
    v-for="task in tasks"
    :key="task.id"
    :task="task"
  />
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Task, { IReminder } from "../components/Task.vue";
import AddTask from "../components/AddTask.vue";

@Options({
  props: {
    showAddTask: {
      type: Boolean
    }
  },
  components: {
    Task,
    AddTask
  },
  data() {
    return {
      tasks: Object as () => IReminder[]
    };
  },
  created() {
    this.fetchTasks()
      .then((data: IReminder[]) => {
        this.tasks = data;
      })
      .catch((erroApi: Error) => {
        console.log(erroApi);
      });
  },
  methods: {
    async deleteTask(id: number) {
      try {
        await fetch(`api/tasks/${id}`, {
          method: "DELETE",
          headers: {
            "Content-type": "application/json"
          }
        });

        this.tasks = this.tasks.filter((task: IReminder) => task.id !== id);
      } catch (errorApi) {
        console.log(errorApi);
      }
    },

    async toggleReminder(taskSelected: IReminder) {
      taskSelected.reminder = !taskSelected.reminder;

      try {
        const res = await fetch(`api/tasks/${taskSelected.id}`, {
          method: "PUT",
          headers: {
            "Content-type": "application/json"
          },
          body: JSON.stringify(taskSelected)
        });

        try {
          const data = await res.json();

          this.tasks = this.tasks.map((task: IReminder) =>
            task.id === taskSelected.id ? data : task
          );
        } catch (errorJson) {
          console.log(errorJson);
        }
      } catch (errorApi) {
        console.log(errorApi);
      }
    },

    async addTask(task: IReminder) {
      try {
        const res = await fetch("api/tasks", {
          method: "POST",
          headers: {
            "Content-type": "application/json"
          },
          body: JSON.stringify(task)
        });

        try {
          const data = await res.json();

          this.tasks.push(data);
        } catch (errorJson) {
          console.log(errorJson);
        }
      } catch (errorApi) {
        console.log(errorApi);
      }
    },

    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();

      return data;
    },
  }
})
export default class Home extends Vue {}
</script>

<style lang="scss" scoped></style>
