<script setup>
import ProductCard from './ProductCard.vue'
import { defineProps, onMounted, reactive, computed, ref, watch } from 'vue'
import { RouterLink } from 'vue-router'
import { BSpinner, BPagination } from 'bootstrap-vue-next'
import axios from 'axios'

defineProps({
  limit: Number,
  showButton: {
    type: Boolean,
    default: false
  },
})

const api_url = 'https://mockapi.io/api/products'
const currentPage = ref(1)
const perPage = 8

const status = reactive({
  products: [],
  loading: true,
})

const pageCount = computed(() => {
  return Math.ceil(status.products.length / perPage)
})

const paginatedProducts = computed(() => {
  const startIndex = (currentPage.value - 1) * perPage
  const endIndex = startIndex + perPage
  return status.products.slice(startIndex, endIndex)
})

onMounted(async () => {
  try {
    const response = await axios.get(api_url)
    status.products = response.data
  } catch (error) {
    console.error('Hämta produkter. Misslyckades ', error)
  }
  finally {
    status.loading = false;
  }
})

watch(currentPage, () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
})

</script>

<template>
  <section class="container px-4 pb-5">
    <div class="container-lg mx-auto">
      <h4 class="fw-bold text-light mb-4 text-center">
        Produkter
      </h4>
      <div class="row g-2">
        <div v-if="status.loading" class="text-center">
          <b-spinner variant="light" label="loading..."></b-spinner>
        </div>
        <div class="col-md-3 d-flex flex-column h-100"
          v-for="product in (limit ? status.products.slice(0, limit) : paginatedProducts)"
          :key="product.id">
          <ProductCard :product="product" />
        </div>
      </div>
    </div>
  </section>

  <section v-if="!limit && pageCount > 1"
    class="d-flex justify-content-center mb-4">
    <b-pagination v-model="currentPage" :total-rows="status.products.length"
      :per-page="perPage" aria-controls="products-table" align="center"
      class="pagination-custom"></b-pagination>
  </section>

  <section class="d-flex">
    <div v-if="showButton" class="mx-auto">
      <RouterLink to="/products">
        <button class="btn btn-primary w-100 py-2 px-5 mb-5">Visa alla
          produkter</button>
      </RouterLink>
    </div>
  </section>
</template>

<style scoped>
#show-all-button
{
  background: #f8f9fa;
}

#show-all-button:hover
{
  background: #d3d5d7;
}


.pagination-custom :deep(.page-link)
{
  color: #bb81f8;
  background-color: transparent;
  border-color: #484848;
}

.pagination-custom :deep(.page-item.active .page-link)
{
  background-color: #bb81f8;
  border-color: #0065fc;
  color: white;
}

.pagination-custom :deep(.page-link:hover)
{
  background-color: #9a67ea;
  color: white;
}

#section
{
  width: 1300px;
}

.btn-primary
{
  background-color: #bb81f8;
}

.btn-primary:hover
{
  background-color: #9a67ea;
}
</style>