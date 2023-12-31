<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <SearchPage></SearchPage>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import SearchPage from '@/components/SearchPage.vue'


export default {
  name: 'HomeView',
  components: {
    HelloWorld,
    SearchPage
  }
}
</script>
<style lang="scss" scoped>
.home{
  width: 100%;
  padding: 0;
  margin: 0;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-sizing: border-box;
}
</style>