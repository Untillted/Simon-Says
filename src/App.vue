<template>
  <div id="app">
    <Simon />
  </div>
</template>

<script>
import Simon from "@/components/Simon.vue";
export default {
  name: "App",
  components: {
    Simon,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 2em;
  cursor: default;
}
h1 {
  margin: 1em 0 2em;
  text-align: center;
}

ul {
  list-style: none;
}

ul,
li {
  padding: 0;
  margin: 0;
}
</style>
