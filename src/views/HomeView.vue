<template>
  <div class="content">
    <div class="row">
      <h1>Товары</h1>
      <div class="cart-row">
        <router-link to="/cart">
          <img src="../assets/cart.png" alt="Cart logo">
        </router-link>
      </div>
    </div>
    <div class="filters">
        <select v-model="selectedCategory" @change="filterProducts">
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

    <div v-if="!loading" class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">←</button>
      <div class="page-nubmers">
      <button 
        v-for="page in totalPages" 
        :key="page" 
        @click="goToPage(page)"
        :class="{ active: currentPage === page}"
      >
        {{ page }}
      </button>
      </div>
      <button @click="nextPage" :disabled="cuurentPage === totalPages">→</button>
      
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
      this.currentPage = 1
      this.loading = false
    },
    filterProducts() {
      this.currentPage = 1
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
    },
    goToPage(page) {
      this.currentPage = page
    }
  },
  mounted() {
    this.fetchProducts()
  }
}
</script>

<style>
.row {
  display: flex;
  align-items: center;
  margin-left: 15px;
}

.cart-row {
  margin-left: 70%;
}

.cart-row img {
  width: 36px;
}

.filters {
  margin-left: 15px;
}

.loading-title {
  display: flex;
  justify-content: center;
}

.product-grid {
  display: flex;
  flex-wrap: wrap;
}

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.pagination button {
  border: 1px solid #ccc;
  color: #2c3e50;
  font-size: 25px;
  margin: 0 5px;
  background-color: transparent;
}

.pagination button.active {
  background-color: rgb(221, 221, 221);
}

.page-numbers {
  display: flex;
}

.page-numbers button {
  margin: 0 2px;
}

@media (max-width: 400px) {
  .product-grid {
    flex-wrap: nowrap;
    flex-direction: column;
  }

  .cart-row {
  margin-left: 50%;
  }

  .pagination {
    flex-wrap: wrap;
  }

  .pagination button {
    font-size: 20px;
    padding: 10px;
  }

  .page-numbers {
    flex-wrap: wrap;
  }

  .page-numbers button {
    margin: 5px;
  }
}
</style>