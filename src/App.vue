<template>
  <HomePage/>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import HomePage from './components/HomePage.vue';


export default defineComponent({
  name: 'App',
  components: {
    HomePage,
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--black);
}
</style>
