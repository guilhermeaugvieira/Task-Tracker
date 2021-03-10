<template>
  <header>
    <h1>
      {{ title }}
    </h1>
    <Button
      v-show="homePage"
      @btn-click="$emit('toggle-add-task')"
      :text="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'"
    />
  </header>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Button from "./Button.vue";

@Options({
  components: {
    Button
  },
  props: {
    title: {
      type: String,
      default: "Task Tracker"
    },
    showAddTask: {
      type: Boolean
    }
  },
  computed: {
    homePage() {
      if (this.$route.path === "/") {
        return true;
      } else {
        return false;
      }
    }
  }
})
export default class Header extends Vue {}
</script>

<style lang="scss" scoped>
header {
  display: flex;
  justify-content: space-between;
  align-content: center;
  margin-bottom: 20px;
}
</style>
