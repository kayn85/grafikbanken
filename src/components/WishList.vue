<script setup>
import { ref, defineEmits } from "vue";

const wishList = ref(JSON.parse(sessionStorage.getItem("wishList")) || []);

const emit = defineEmits(["updateWishList"]);

function removeFromWishList(product) {
  wishList.value = wishList.value.filter((wiLs) => wiLs.id !== product.id);
  sessionStorage.setItem("wishList", JSON.stringify(wishList.value));
  emit("updateWishList", wishList.value);
}
</script>

<template>
  <table class="table table-dark m-0">
    <thead class="">
      <tr>
        <th>Produkt</th>
        <th>Pris</th>
        <th>Ta bort från önskelista</th>
      </tr>
    </thead>
    <tbody class="table-group-divider">
      <tr v-for="product in wishList" :key="product.id"
        class="rounded shadow-md position-relative mb-4 bg-dark">
        <td class="d-flex align-items-center gap-2 border-0">
          <img :src="product.bild" :alt="product.title"
            class="image-thumbnail d-none d-sm-block" style="width: 90px" />
          <RouterLink :to="'products/' + product.id"
            class="text-white text-decoration-none">
            <p class="m-0">{{ product.title }}</p>
          </RouterLink>
        </td>
        <td class="align-middle border-0">
          <p class="m-0 text-info fw-bold">{{ product.pris }} kr</p>
        </td>
        <td class="align-middle border-0">
          <button class="btn btn-danger "
            @click="removeFromWishList(product)">Ta
            bort</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
.text-info
{
  color: #bb81f8 !important;
}
</style>
