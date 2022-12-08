<template>
  <Menu />
  <router-view />
</template>

<script>
import Menu from './components/Menu.vue';
import { onMounted } from 'vue';

export default {
  components: {
    Menu
  },
  setup() {
    onMounted( async () => {

    })
  }
}
</script>

<style>
body {
  background: #f1f1f1;
  font-size: 16px;
}
.card {
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
}

.card:hover {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
}
</style>