<template>
  <header>
    <h1>
      {{ title }}
    </h1>
    <Button color="green" />
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
