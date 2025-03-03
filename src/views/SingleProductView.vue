<script setup>
import { reactive, onMounted, ref } from "vue";
import { useRoute, RouterLink, useRouter } from "vue-router";
import { useToast } from 'vue-toast-notification'
import 'vue-toast-notification/dist/theme-sugar.css'
import axios from "axios";

const toast = useToast({
  position: 'top-right'
})

const route = useRoute();
const productId = route.params.id;

const status = reactive({
  product: {},
});

const api_url = "https://mockapi.io/api/products";

onMounted(async () => {
  try {
    const response = await axios.get(`${api_url}/${productId}`);
    status.product = response.data;
  } catch (error) {
    console.error("Gick ej att hämta produkt", error);
  }
});

function addToCart() {
  let cart = []
  if (JSON.parse(localStorage.getItem('cart'))) {
    cart = JSON.parse(localStorage.getItem('cart'))
  }

  cart.push({
    id: status.product.id,
    title: status.product.title,
    pris: status.product.pris,
    bild: status.product.bild
  })

  localStorage.setItem('cart', JSON.stringify(cart))
  toast.success('Produkt har lagts till')

}

function saveToWishList() {
  let wishList = JSON.parse(sessionStorage.getItem("wishList")) || [];
  if (!wishList.some((wiLs) => wiLs.id === status.product.id)) {
    wishList.push(status.product);
    sessionStorage.setItem("wishList", JSON.stringify(wishList));
  }
  toast.success('Finns på önskelistan')
}
</script>

<template>
  <section class="product-section d-flex flex-column">
    <div class="container py-4 flex-grow-1">
      <div class="row">
        <main class="col-md-8">
          <div class="card mb-3 product-card">
            <div class="d-flex justify-content-center my-3">
              <img :src="status.product.bild" :alt="status.product.title"
                class="img-fluid">
            </div>

          </div>
        </main>

        <aside class="col-md-4">

          <div class="card mb-3 product-card">
            <div class="card-body">
              <h5 class="text-light">Artikelnr: {{ status.product.artnr }}</h5>
              <h4 class="text-white">{{ status.product.title }}</h4>
            </div>
          </div>

          <div class="card mb-3 product-card">
            <div class="card-body">
              <h3 class="text-white">Beskrivning:</h3>
              <p class="text-light">{{ status.product.beskrivning }}</p>
            </div>
          </div>
          <div class="card product-card">
            <div class="card-body text-center">
              <h3 class="text-primary">{{ status.product.pris }} kr</h3>

              <button class="btn btn-danger w-100 mb-2 p-12"
                @click="saveToWishList">
                <i class="bi bi-heart-fill"></i> Önskelista
              </button>
              <button class="btn btn-primary w-100" @click="addToCart"><i
                  class="bi bi-bag"></i> Köp</button>
            </div>
          </div>
        </aside>
      </div>
    </div>
  </section>
</template>

<style scoped>
.product-section
{
  background: #121212;
  color: #ffffff;
  padding: 2rem 0;
}

.product-card
{
  background: #1e1e1e;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  border: none;
}

.img-fluid
{
  max-width: 100%;
  height: 500px;
  border-radius: 8px;
  object-fit: contain;
}

.text-light
{
  color: #b0b0b0 !important;
}

.text-grey
{
  color: #757575 !important;
}

.btn-primary
{
  background: #bb81f8;
  border: none;
  padding: 12px;
  font-size: 1.1rem;
  border-radius: 8px;
}

.text-primary
{
  color: #bb81f8 !important;
}

.btn-primary:hover
{
  background: #975dd6;
}

#bg
{
  background: #413d46;
}

.btn-danger
{
  border: none;
  padding: 12px;
  font-size: 1.1rem;
  border-radius: 8px;
}
</style>
