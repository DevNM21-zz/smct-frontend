<template>
  <div id="app">
    <NavBar></NavBar>
    <ListHeader></ListHeader>
    <List ref="list"></List>
  </div>
</template>

<script>
import List from "./components/List.vue";
import NavBar from "./components/NavBar.vue";
import ListHeader from "./components/ListHeader.vue";

export default {
  name: "app",
  components: {
    List,
    ListHeader,
    NavBar
  }
  // mounted() {
  // }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
