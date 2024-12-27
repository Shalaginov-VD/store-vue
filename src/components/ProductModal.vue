<template>
    <div class="modal" v-if="isVisible">
        <div class="modal-content">
            <div v-if="message" class="notification">{{ message }}</div>
            <span class="close" @click="closeModal">&times;</span>
            <img class="product-img" :src="product.image" alt="Product image" />
            <h2>{{ product.title }}</h2>
            <p>Цена: {{ convertToRub(product.price) }} ₽</p>
            <p>Категория: {{ product.category }}</p>
            <p>Описание: {{ product.description }}</p>
            <p>Оценка: {{ product.rating.rate }} ({{ product.rating.count }} отзывов)</p>
            <button @click.stop="addToCart">Добавить в корзину</button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        product: Object,
        isVisible: Boolean
    },
    data() {
        return {
            message: ''
        }
    },
    methods: {
        closeModal() {
            this.$emit('close')
        },
        convertToRub(price) {
            const exchangeRate = 100
            return price * exchangeRate
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

.modal-content button {
    border: 1px solid #ccc;
    background: none;
    color: #2c3e50;
    width: 100%;
    font-size: 16px;
    padding: 10px;
    cursor: pointer;
}

.modal-content button:hover {
    background-color: rgb(221, 221, 221)
}

@media (max-width: 400px) {
  .modal-content {
    max-width: 500px;
    font-size: 12px;
  }
}
</style>