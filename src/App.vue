<template>
  <div id="app">
    <leftNav></leftNav>
    <div class="main">
        <router-view/>
    </div>
  </div>
</template>

<script>
import leftNav from '@/components/common/LeftNav'
export default {
  name: 'App',
  components: {
    leftNav
  }
}
</script>

<style>
  .main{
    width: 95%;
    float: left;
    background-color: #eff2f7;
    height: 100%;
    overflow: hidden;
    text-align: center;
  }
</style>
