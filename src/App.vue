<script setup>
  import Main from './components/Main.vue'
</script>

<template>
  <div class="app-wrapper">
    <Main />
  </div>
</template>

<style scoped>
  .app-wrapper{
    width: 100vw;
    height: 100vh;
    display: flex;
    background-color: var(--vt-c-indigo);
  }

  * ::-webkit-scrollbar{
    width: 8px;
    height: 8px;
    background-color: var(--background-dark);
    
  }
  * ::-webkit-scrollbar-thumb{
    background-color: var(--background-light);
    border: 2px solid var(--background-dark);
    border-radius: 3px;
  }
  * ::-webkit-scrollbar-corner{background-color: var(--background-dark);}
</style>
