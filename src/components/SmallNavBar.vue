<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
  <nav class="navbar navbar-dark navbar-expand-lg">
    <div class="container-lg d-flex align-items-center justify-content-center">
      <div
        class="navbar-nav d-flex justify-content-center align-items-center mx-auto gap-md-5 "
        style="max-width: 1300px; width: 100%;">
        <RouterLink to="/" class="nav-link text-white fw-semibold p-2 px-3">
          <i class="bi bi-house-door-fill"></i> Hem
        </RouterLink>
        <RouterLink to="/products"
          class="nav-link text-white fw-semibold p-2 px-3">
          Produkter
        </RouterLink>
        <RouterLink to="/sell" class="nav-link text-white fw-semibold p-2 px-3">
          Sälj
        </RouterLink>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar-nav
{
  background-color: #333333;
  border-radius: 6px;
  width: 100%;
  max-width: 1300px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}

.nav-link:hover
{
  background-color: #bb81f8;
  border-radius: 6px;
}

@media (max-width: 992px)
{
  .navbar-nav
  {
    flex-direction: column;
    align-items: center;
  }
}
</style>
