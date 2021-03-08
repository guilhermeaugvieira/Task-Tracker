<template>
  <div :class="[task.reminder ? 'reminder' : '', 'task']">
    <h3>
      {{ task.text }}
      <i @click="onDelete(task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

export type IReminder = {
  id: number;
  text: string;
  day: string;
  reminder: boolean;
};

@Options({
  props: {
    task: {
      type: Object as () => IReminder
    }
  },
  methods: {
    onDelete(id: number) {
      this.$emit('delete-task', id);
    }
  }
})
export default class Task extends Vue {}
</script>

<style lang="scss" scoped>
.task {
  background-color: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;

  h3 {
    display: flex;
    align-items: center;
    justify-content: space-between;

    i{
      cursor: pointer;

      &.fas {
      color: red;
    }
    }
  }

  &.reminder {
    border-left: 5px solid green;
  }
}


</style>
