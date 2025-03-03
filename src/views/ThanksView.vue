<script setup>
import { RouterLink } from 'vue-router';


</script>

<template>
  <section class="container">
    <div
      class="bg-dark text-light d-flex justify-content-center align-items-center mb-5">
      <div class="text-center">
        <div class="pb-3">
          <i class="bi bi-check-circle h1"></i>
        </div>
        <h1>Tack för din beställning!</h1>
        <p>Vi uppskattar ditt köp och hoppas att du njuter av produkten.</p>
        <RouterLink to="/">
          <button class="btn btn-primary">Gå tillbaka till hem</button>
        </RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
.bg-dark
{
  background-color: #1e1e1e !important;
  height: 400px;
  border-radius: 6px;
}

.btn-primary
{
  background-color: #bb81f8;

}

.btn-primary:hover
{
  background-color: #9a67ea;
}

i
{
  color: #bb81f8;
}
</style>