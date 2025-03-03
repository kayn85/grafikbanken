<script setup>
import { defineProps } from 'vue';
import { RouterLink } from 'vue-router';

const props = defineProps({
    product: {
        type: Object,
        required: true
    }
});
</script>

<template>
    <div class="rounded shadow-md position-relative product-card">
        <div class="p-3 d-flex flex-column">
            <RouterLink :to="'products/' + product.id">
                <div class="col-12">
                    <img :src="product.bild" :alt="product.title"
                      class="img-fluid rounded">
                </div>
            </RouterLink>
            <h3 class="fs-6 fw-bold text-white text-center text-truncate">{{
                product.title }}</h3>
            <div class="d-flex justify-content-end mt-2">
                <div class="text-primary fw-bold">{{ product.pris }} kr</div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.product-card
{
    background: #1e1e1e;
    border-radius: 12px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    border: none;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100%;
}

.img-fluid
{
    width: 100%;
    height: 110px;
    border-radius: 8px;
    object-fit: contain;
}

.text-grey
{
    color: #757575 !important;
}

.btn-primary
{
    background-color: #bb81f8;
    border: none;
    padding: 12px;
    font-size: 1.1rem;
    border-radius: 8px;
}

.btn-primary:hover
{
    background-color: #9a67ea;
}

.text-light
{
    color: #b0b0b0 !important;
}

.text-primary
{
    color: #bb81f8 !important;
}
</style>
