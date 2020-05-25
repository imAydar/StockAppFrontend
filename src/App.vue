<template>
  <div id="app">
    <h1 id="nav">
      <router-link to="/">Документы</router-link> 
    </h1>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color:#41a37a;
 
}

#nav {
 /*padding: 30px;*/
}

#nav a {
  font-weight: bold;
 /* color: #2c3e50;*/
  text-decoration: unset;
 
}
html{
   background-color: #3a4254;
}
/*#nav a.router-link-exact-active */#nav a{
  /*color: #42b983;*/
   color:white;
}
</style>
