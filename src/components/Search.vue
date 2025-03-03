<script setup>
import { ref, watch, computed } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import axios from 'axios';

const router = useRouter();
const route = useRoute();

const searchQuery = ref('');
const searchResults = ref([]);
const isLoading = ref(false);
const showResults = ref(false);

// begränsad sökning till 5
const limitedResults = computed(() => searchResults.value.slice(0, 5));

// kollar förändringar i sökfältet
watch(searchQuery, async (newQuery) => {
  if (newQuery.length < 2) {
    searchResults.value = [];
    showResults.value = false;
    return;
  }

  isLoading.value = true;
  try {
    const response = await axios.get('https://mockapi.io/api/products');
    const products = response.data;

    searchResults.value = products.filter(product =>
      product.title.toLowerCase().includes(newQuery.toLowerCase())
    );

    showResults.value = searchResults.value.length > 0;
  } catch (error) {
    console.error('Fel vid sökning:', error);
    searchResults.value = [];
  } finally {
    isLoading.value = false;
  }
});

// går till produkt och återställ sökning
const goToProduct = async (productId) => {
  showResults.value = false;

  if (route.path === `/products/${productId}`) {
    await router.replace(`/products/${productId}`);
  } else {
    await router.push(`/products/${productId}`);
  }

  searchQuery.value = '';
};

// klicka utanför för att avbryta sökning
const handleClickOutside = () => {
  showResults.value = false;
};

// använd esc knappen för att avbryta
const handleKeyDown = (event) => {
  if (event.key === 'Escape') {
    showResults.value = false;
  }
};

// Återställ sökning vid när sidan byte
watch(route, () => {
  searchQuery.value = '';
  showResults.value = false;
});
</script>

<template>
  <div class="search-container">
    <div class="search-input-container">
      <input v-model="searchQuery" class="form-control" type="search"
        placeholder="Sök produkter..." aria-label="Search"
        @focus="showResults = searchResults.length > 0"
        @keydown="handleKeyDown" />
      <button class="btn btn-outline-info" type="button">
        <i class="bi bi-search"></i>
      </button>
    </div>

    <div v-if="showResults && limitedResults.length > 0" class="search-results">
      <div v-for="product in limitedResults" :key="product.id"
        class="search-result-item" @click="goToProduct(product.id)">
        <div class="search-result-info">
          <div class="search-result-title">{{ product.title }}</div>
        </div>
      </div>
    </div>

    <div v-if="isLoading" class="search-loading">
      Laddar...
    </div>

    <div
      v-if="showResults && searchQuery.length >= 2 && limitedResults.length === 0"
      class="search-no-results">
      Inga produkter hittades
    </div>

    <div v-if="showResults" class="search-overlay" @click="handleClickOutside">
    </div>
  </div>
</template>

<style scoped>
.search-container
{
  position: relative;
  width: 100%;
}

.search-input-container
{
  display: flex;
  width: 100%;
}

.search-results
{
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background-color: #1e1e1e;
  color: rgb(255, 255, 255);
  border-radius: 6px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
  z-index: 1000;
}

.search-result-item
{
  padding: 10px;
  cursor: pointer;
  border-bottom: 1px solid #2c2c2c;
  transition: background-color 0.2s;
}

.search-result-item:hover
{
  background-color: #2a2a2a;
}

.search-result-title
{
  font-weight: bold;
}

.search-loading,
.search-no-results
{
  padding: 10px;
  text-align: center;
  background-color: #1e1e1e;
  color: rgb(224, 211, 211);
  border-radius: 5px;
  position: absolute;
  width: 100%;
  top: 100%;
  z-index: 1000;
}

.search-overlay
{
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 999;
}

.btn-outline-info
{
  color: #bb81f8;
}

i
{
  color: white;
}
</style>
