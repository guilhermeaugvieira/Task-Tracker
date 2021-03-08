<template>
  <button @click="onClick()" :style="{ backgroundColor: color }" class="btn">
    {{ text }}
  </button>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

@Options({
  props: {
    text: {
      type: String,
      default: "Add Task"
    },
    color: {
      type: String,
      default: "#000"
    }
  },
  methods: {
    onClick() {
      console.log("click");
    }
  }
})
export default class Button extends Vue {}
</script>

<style lang="scss" scoped></style>
