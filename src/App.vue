<script setup>
import { RouterView } from "vue-router";
import Footer from "./components/Footer.vue";
import NavBar from "./components/NavBar.vue";
import SmallNavBar from "./components/SmallNavBar.vue";
</script>

<template>
  <NavBar />
  <SmallNavBar />
  <div class="content">
    <!-- tvinga vue att omrenderar även om (id) ändras. för search.vue -->
    <RouterView :key="$route.fullPath" />
  </div>
  <Footer />
</template>

<style>
html,
body
{
  height: 100vh;
  margin: 0;
  padding: 0;
}

#bg
{
  background: #121212;
  color: #ffffff;
  padding: 2rem 0;
}

#app
{
  display: flex;
  flex-direction: column;
  font-family: "Roboto", sans-serif;
  min-height: 100vh;
  background: #121212;
}

.content
{
  flex-grow: 1;
  margin-top: 20px;
}
</style>
