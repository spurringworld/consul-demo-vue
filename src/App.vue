<template>
  <div id="app">
    <el-container>
      <el-header>
        <h1>Consul Demo</h1>
      </el-header>
      <el-main>
        <router-view/>
      </el-main>
      <el-footer>
        <br />
        <el-link type="info" href="https://flomesh.io/" target="_blank">flomesh.io</el-link>
      </el-footer>
    </el-container>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>