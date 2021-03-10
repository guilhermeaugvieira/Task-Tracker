<template>
  <footer>
    <p>Copyright &copy; 2021</p>
    <router-link to="/about">About</router-link>
  </footer>
</template>

<script lang="ts">
import { Options, Vue} from "vue-class-component";

@Options({
})
export default class Footer extends Vue {}
</script>

<style lang="scss" scoped>
footer {
  margin-top: 30px;
  text-align: center;

  a {
    color: #333;
  }
}
</style>