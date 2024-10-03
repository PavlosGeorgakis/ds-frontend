<template>
  <div>
    <Navbar />
    <div id="app">
      <router-view></router-view>
    </div>
    <FooterBar />
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
};
</script>

<style>
div#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
