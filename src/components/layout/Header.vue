<template>
  <header class="header">
    <h1>Coffee Run</h1>
    <span>Power by</span>
    <span>
      <img src="./assets/logo_foursquare8-50f95c75a6ba64c854bb3c2acf24c96c.png" alt="">
    </span>
  </header>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style scoped>
  .header {
    background: #00113D;
    color: #fff;
    text-align: center;
    padding: 10px;
    margin-bottom: 15px;
  }
  img {
    height: 15px;
  }
  span {
    font-size: 10px;
    margin-right: 10px;
  }
</style>

