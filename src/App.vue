<template>
  <div id="app">
    <div id="nav">
      <tp-nav-bar/>
    </div>
  </div>
  <router-view/>
</template>

<script>
  import NavBarTP from "@/components/NavBarTP.vue";
  export default{
    components: {
      "tp-nav-bar": NavBarTP
    }
  }
</script>
