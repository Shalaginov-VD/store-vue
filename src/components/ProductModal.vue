<template>
    <div class="modal" v-if="isVisible">
        <div class="modal-content">
            <span class="close" @click="closeModal">&times;</span>
            <img class="product-img" :src="product.image" alt="Product image" />
            <h2>{{ product.title }}</h2>
            <p>Цена: {{ convertToRub(product.price) }} ₽</p>
            <p>Категория: {{ product.category }}</p>
            <p>Описание: {{ product.description }}</p>
            <p>Оценка: {{ product.rating.rate }} ({{ product.rating.count }} отзывов)</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        product: Object,
        isVisible: Boolean
    },
    methods: {
        closeModal() {
            this.$emit('close')
        },
        convertToRub(price) {
            const exchangeRate = 100
            return price * exchangeRate
        }
    }
}
</script>

<style>
.modal {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 5px;
    position: relative;
    width: 70%;
}

.close {
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
    font-size: 24px;
}

.product-img {
    width: 25%;
}
</style>