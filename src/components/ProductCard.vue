<template>
    <div v-if="message" class="notification">{{ message }}</div>
    <div class="product-card" @click="handleClick">
        <img class="product-img" :src="product.image" alt="Product image">
        <h2>{{ product.title }}</h2>
        <p>Цена: {{ convertToRub(product.price) }} ₽</p>
        <p>Категория: {{ product.category }}</p>
        <p>Оценка: {{ product.rating.rate }}</p>
        <button @click.stop="addToCart">Добавить в корзину</button>
    </div>
</template>

<script>
export default {
    props: {
        product: Object
    },
    data() {
        return {
            message: ''
        }
    },
    methods: {
        convertToRub(price) {
            const exchangeRate = 100;
            return price * exchangeRate;
        },
        addToCart() {
            const cartItem = {
                id: this.product.id,
                title: this.product.title,
                price: this.product.price,
                quantity: 1
            }

            let cart = JSON.parse(localStorage.getItem('cart')) || []
            const existingItemIndex = cart.findIndex(item => item.id === cartItem.id)

            if (existingItemIndex > -1) {
                cart[existingItemIndex].quantity +=1
            } else {
                cart.push(cartItem)
            }

            localStorage.setItem('cart', JSON.stringify(cart))
            this.message = 'Товар добавлен в корзину'
            setTimeout(() => {
                this.message = ''
            }, 3000)
        },
        handleClick() {
            this.$emit('click')
        }
    }
}
</script>

<style>
.filters select {
    border: 1px solid #ccc;
    color: #2c3e50;
    font-size: 16px;
}

.product-card {
    border: 1px solid #ccc;
    width: 25%;
    padding: 15px;
    margin: 15px;
}

.product-card button {
    border: 1px solid #ccc;
    background: none;
    color: #2c3e50;
    width: 100%;
    font-size: 16px;
    padding: 10px;
    cursor: pointer;
}

.product-card button:hover {
    background-color: rgb(221, 221, 221)
}

.product-img {
    width: 25%;
}

.notification {
    background-color: #4CAF50;
    color: white;
    padding: 10px;
    position: fixed;
    bottom: 20px;
    right: 20px;
    border-radius: 5px;
    z-index: 1000;
}

@media (max-width: 400px) {
  .product-card {
    width: 85%;
    max-width: 350px;
  }
}
</style>