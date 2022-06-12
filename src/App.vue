<template>
<Navbar></Navbar>
  <div  class="wrapper">
    <router-view></router-view>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
export default {
    components: { Navbar }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.wrapper {
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  width: 1000px;
}

</style>