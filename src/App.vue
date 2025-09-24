<template>
  <div id="app-wrapper">
    <Sidebar v-if="$route.name !== 'Login'" />
    <main class="main-content">
      <router-view/>
    </main>
  </div>
</template>

<script>
import Sidebar from './components/Sidebar.vue';

export default {
  name: 'App',
  components: {
    Sidebar
  }
};
</script>

<style>
body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f0f2f5;
}

#app-wrapper {
  display: flex;
}

.main-content {
  flex-grow: 1;
  padding: 30px;
  margin-left: 250px;
}

.main-content:has(> .login-page) {
  margin-left: 0;
  padding: 0;
}
</style>
