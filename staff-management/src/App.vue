<template>
  <div id="app">
    <AdminStaff v-show="true"></AdminStaff>
    <ToSearch v-show="false"/>
    <MyList v-show="false"/>
    <router-view v-if="isRouterAlive"/>
  </div>
</template>

<script>
import AdminStaff from '@/views/AdminStaff'
import MyList from "@/components/MyList";
import ToSearch from "@/components/ToSearch";

export default {
  name: 'App',
  provide () {
    return {
      reload: this.reload
    };
  },
  methods: {
    reload () {
      this.isRouterAlive = false
      this.$nextTick(function () {
        this.isRouterAlive = true
      });
    }
  },
  components: {
    AdminStaff,
    MyList,
    ToSearch,
  }
}  
</script>
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
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
