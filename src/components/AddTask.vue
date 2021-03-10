<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input
        type="text"
        v-model="task.text"
        name="text"
        placeholder="Add Task"
      />
    </div>
    <div class="form-control">
      <label>Day & Time</label>
      <input
        type="text"
        v-model="task.day"
        name="day"
        placeholder="Add Day & Time"
      />
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="task.reminder" name="reminder" />
    </div>
    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { IReminder } from "./Task.vue";

@Options({
  data() {
    return {
      task: {
        type: Object as () => IReminder
      }
    };
  },
  created() {
    this.task = {};
  },
  methods: {
    onSubmit(e: Event) {
      e.preventDefault();

      if (!this.task.text) {
        alert("Please add a task");
        return;
      }

      if (this.task.reminder === undefined) this.task.reminder = false;

      this.$emit("add-task", this.task);
    }
  }
})
export default class AddTask extends Vue {}
</script>

<style lang="scss" scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;

  label {
    display: block;
  }

  input {
    width: 100%;
    height: 40px;
    margin: 5px;
    padding: 3px 7px;
    font-size: 17px;
  }

  &.form-control-check {
    display: flex;
    align-items: center;
    justify-content: space-between;

    label {
      flex: 1;
    }

    input {
      flex: 2;
      height: 20px;
    }
  }
}
</style>
