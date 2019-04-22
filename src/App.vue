<template>
  <div id="app">
    <!-- SET COMPONENTS -->
    <Header/>
    <SearchCity/>
  </div>
</template>

<script>
// IMPORT COMPONENTS
import Header from "./components/layout/Header";
import SearchCity from "./components/SearchCity";
import axios from "axios";

export default {
  name: "app",
  components: {
    Header,
    SearchCity
  }
};
</script>

//CSS
<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
</style>
  