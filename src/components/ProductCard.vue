<template>
    <div class="product-card">
        <img class="product-img" :src="product.image" alt="Product image">
        <h2>{{ product.title }}</h2>
        <p>Цена: {{ convertToRub(product.price) }} ₽</p>
        <p>Категория: {{ product.category }}</p>
        <p>
            Оценка: {{ product.rating.rate }}
        </p>
    </div>
</template>

<script>
export default {
    props: {
        product: Object
    },
    methods: {
        convertToRub(price) {
            const exchangeRate = 100;
            return price * exchangeRate;
        }
    }
}
</script>

<style>
.product-card {
    border: 1px solid #ccc;
    width: 25%;
    padding: 15px;
    margin: 15px;
}

.product-img {
    width: 25%;
}
</style>