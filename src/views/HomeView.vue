<template>
  <div class="content">
    <h1>Товары</h1>
    <div class="filters">
      <select v-model="selectedCategory" @change="fetchProducts">
        <option value="">Все категории</option>
        <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
      </select>
    </div>
    <div class="loading-title" v-if="loading">Загрузка...</div>
    <div class="product-grid">   
      <ProductCard
        v-for="product in filteredProducts"
        :key="product.id"
        :product="product"
        @click="openModal(product)"
      />
    </div>

    <ProductModal
      v-if="selectedProduct"
      :product="selectedProduct"
      :isVisible="!!selectedProduct"
      @close="closeModal"
    />
  </div>
</template>

<script>
import ProductCard from '../components/ProductCard.vue'
import ProductModal from '../components/ProductModal.vue'

export default {
  name: 'HomeView',
  components: {
    ProductCard,
    ProductModal
  },
  data() {
    return {
      loading: true,
      products: [],
      selectedCategory: '',
      categories: [],
      selectedProduct: null
    }
  },
  computed: {
    filteredProducts() {
      return this.selectedCategory
        ? this.products.filter(product => product.category === this.selectedCategory)
        : this.products
    }
  },
  methods: {
    async fetchProducts() {
      this.loading = true
      const response = await fetch(`https://fakestoreapi.com/products`)
      this.products = await response.json()
      this.categories = [...new Set(this.products.map(product => product.category))]
      this.loading = false
    },
    openModal(product) {
      this.selectedProduct = product
    },
    closeModal() {
      this.selectedProduct = null
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