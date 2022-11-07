<template>
  <v-app-bar app color="#111111" dark flat class="px-8">
    <v-btn icon @click="scroll('home')" aria-label="Left Align">
      <v-icon color="#A7121D">fas fa-l</v-icon>
      <v-icon color="#A7121D">fas fa-m</v-icon>
    </v-btn>
    <v-spacer></v-spacer>
    <v-btn text aria-label="Left Align">
      <router-link to="/"
        ><v-icon color="#A7121D">fas fa-home</v-icon></router-link
      >
    </v-btn>
    <v-btn text aria-label="Left Align"
      ><router-link to="/history" class="white--text"
        >History</router-link
      ></v-btn
    >
  </v-app-bar>
</template>

<script>
export default {
  name: "NavBar",
  methods: {
    // Scroll
    scroll(refName) {
      const element = document.getElementById(refName);
      element.scrollIntoView({ behavior: "smooth" });
    },
  },
};
</script>
