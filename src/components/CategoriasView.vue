<template>
  <v-container>
    <v-row>
      <v-col v-for="product in paginatedProducts" :key="product.store_product_id" cols="12" sm="6" md="4" lg="3">
        <v-card>
          <v-img :src="product.src" height="200px"></v-img>
          <v-card-title>{{ product.name }}</v-card-title>
          <v-card-subtitle>
            {{ product.categories.map(category => category.name).join(', ') }}
          </v-card-subtitle>
          <v-card-text>
            <div>Tamanhos disponíveis:</div>
            <v-chip-group active-class="primary--text" column>
              <v-chip v-for="variant in product.variants" :key="variant.id">
                {{ variant.value }}
              </v-chip>
            </v-chip-group>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-pagination v-model="currentPage" :length="totalPages" @input="onPageChange"></v-pagination>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CategoriasView',
  data() {
    return {
      products: [],
      currentPage: 1,
      itemsPerPage: 8,
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.products.length / this.itemsPerPage);
    },
    paginatedProducts() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.products.slice(start, end);
    }
  },
  methods: {
    fetchProducts(searchQuery = '') {
      axios.post('https://apibgdev.nswebservice.com.br/store/10/products/', {
        name: searchQuery // Envia o termo de busca pelo nome do produto
      })
        .then(response => {
          this.products = response.data;
          this.currentPage = 1; // Reset pagination on new search
        })
        .catch(error => {
          console.error('Erro ao buscar produtos:', error);
        });
    },
    onPageChange(page) {
      this.currentPage = page;
    }
  },
  watch: {
    $route(to, from) {
      this.fetchProducts(to.query.q || '');
    }
  },
  created() {
    this.fetchProducts(this.$route.query.q || '');
  }
};
</script>

<style>
.v-card {
  margin: 1rem 0;
}
</style>
