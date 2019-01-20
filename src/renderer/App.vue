<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: 'app',
  };
</script>

<style>
  /* CSS */
  html {
	  overflow: hidden;
  }
  body {
	  margin: 0;
	  padding: 0;
	  width: 100%;
	  font-family: Roboto;
	  font-weight: 300;
	  display: -webkit-box;
	  display: flex;
	  -webkit-box-align: center;
	  align-items: center;
	  -webkit-box-pack: center;
	  justify-content: center;
  }
</style>
