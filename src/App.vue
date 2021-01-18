<template>
  <v-app>
    <v-main>
      <HomeController />
    </v-main>
  </v-app>
</template>

<script>
import HomeController from "./components/HomeController";

export default {
  name: "App",

  components: {
    HomeController,
  },

  data: () => ({
    //
  }),
};
</script>
