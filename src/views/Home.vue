<template>
  <div class="home">
    <HomeController></HomeController>
  </div>
</template>

<script>
// @ is an alias to /src
import HomeController from "@/components/HomeController.vue";

export default {
  name: "Home",
  components: {
    HomeController,
  },
  mounted() {
    document.title = "Blackbox";
  },
};
</script>
