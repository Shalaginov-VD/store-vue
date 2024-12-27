<template>
    <div class="cart">
        <h1>Корзина</h1>
        <router-link to="/" class="back-button">Назад</router-link>
        <div class="cart-empty" v-if="cartItems.length === 0">Корзина пуста</div>
        <div class="cart-load" v-else>
            <ul>
                <li v-for="item in cartItems" :key="item.id">
                    {{ item.title }} - {{ item.quantity }} шт. - {{ convertToRub(item.price * item.quantity) }} ₽
                    <button @click="removeFromCart(item.id)">Удалить</button>
                </li>
            </ul>
            <h2>Общая стоимость: {{ (convertToRub(totalPrice)).toFixed(2) }} ₽</h2>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            cartItems: []
        }
    },
    computed: {
        totalPrice() {
            return this.cartItems.reduce((total, item) => total + item.price * item.quantity, 0)
        }
    },
    methods: {
        convertToRub(price) {
            const exchangeRate = 100
            return price * exchangeRate
        },
        loadCart() {
            const cart = JSON.parse(localStorage.getItem('cart')) || []
            this.cartItems = cart
        },
        removeFromCart(productId) {
            this.cartItems = this.cartItems.filter(item => item.id !== productId)
            localStorage.setItem('cart', JSON.stringify(this.cartItems))
        }
    },
    mounted() {
        this.loadCart()
    }
}
</script>

<style>
.cart {
    padding: 20px;
}

.back-button {
    display: inline-block;
    text-decoration: none;
}
</style>