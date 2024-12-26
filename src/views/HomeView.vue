<template>
  <div class="content">
    <h1>Товары</h1>
    <div class="product-grid">
      <div class="loading-title" v-if="loading">Загрузка...</div>
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
      />
    </div>
  </div>
</template>

<script>
import ProductCard from '../components/ProductCard.vue'

export default {
  name: 'HomeView',
  components: {
    ProductCard
  },
  data() {
    return {
      loading: true,
      products: []
    }
  },
  methods: {
    async fetchProducts() {
      const response = await fetch(`https://fakestoreapi.com/products`)
      this.products = await response.json()
      this.loading = false
    }
  },
  mounted() {
    this.fetchProducts()
  }
}
</script>

<style>
.product-grid {
  display: flex;
  flex-wrap: wrap;
}
</style>