<template>
  <div v-for="menu in dailyMenus" :key="menu">
    <img class="menu-img" :src="menu" :alt="'食堂菜单'" loading="lazy">
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'Menu',
  setup() {
    const dailyMenus = ref([
      "https://i.postimg.cc/0Q1JDSBK/1.png",
      "https://i.postimg.cc/wv5RVBCf/2.png",
      "https://i.postimg.cc/brzDFh8z/3.png",
      "https://i.postimg.cc/Ls11JDNM/4.png"
    ]);

    return {
      dailyMenus,
    };
  },
};
</script>

<style scoped>
.menu-img {
  margin-bottom: 2rem;
  max-width: 100%;
  height: auto;
}
</style>
