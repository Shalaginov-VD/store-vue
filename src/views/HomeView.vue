<template>
  <div class="content">
    <h1>Товары</h1>
    <router-link to="/cart">
      <img src="../assets/cart.png" alt="Cart logo">
    </router-link>
    <div class="filters">
      <select v-model="selectedCategory" @change="fetchProducts">
        <option value="">Все категории</option>
        <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
      </select>
    </div>
    <div class="loading-title" v-if="loading">Загрузка...</div>
    <div class="product-grid">   
      <ProductCard
        v-for="product in paginatedProducts"
        :key="product.id"
        :product="product"
        @click="openModal(product)"
      />
    </div>

    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Назад</button>
      <span>Страница {{ currentPage }} из {{ totalPages }}</span>
      <button @click="nextPage" :disabled="cuurentPage === totalPages">Вперед</button>
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
      selectedProduct: null,
      currentPage: 1,
      itemsPerPage: 10
    }
  },
  computed: {
    filteredProducts() {
      return this.selectedCategory
        ? this.products.filter(product => product.category === this.selectedCategory)
        : this.products
    },
    paginatedProducts() {
      const start = (this.currentPage - 1) * this.itemsPerPage
      const end = start + this.itemsPerPage
      return this.filteredProducts.slice(start, end)
    },
    totalPages() {
      return Math.ceil(this.filteredProducts.length / this.itemsPerPage)
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
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage += 1
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage -= 1
      }
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

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
</style>