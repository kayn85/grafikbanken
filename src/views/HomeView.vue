<script setup>
import Hero from '../components/Hero.vue'
import AllProducts from '../components/AllProducts.vue'
</script>

<template>
  <Hero />
  <AllProducts :limit="4" :showButton="true" />
</template>