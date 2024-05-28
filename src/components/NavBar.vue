<template>
  <nav class="navbar">
    <div class="brand">
      <span class="logo">Beeasy</span>
    </div>
    <div class="search">
      <input type="text" v-model="searchQuery" placeholder="Pesquisar...">
      <button @click="search">Buscar</button>
    </div>
  </nav>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'NavBar',
  emits: ['search'],
  setup(_, { emit }) {
    const searchQuery = ref('');

    const search = () => {
      emit('search', searchQuery.value);
    };

    return { searchQuery, search };
  }
}
</script>

<style>
.navbar {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  align-items: center;
  padding: 1rem;
  background-color: #333;
  color: #fff;
}

.brand {
  font-size: 1.5rem;
  text-align: center;
}

.search {
  display: flex;
  justify-content: center;
}

.search input {
  padding: 0.5rem;
  border: none;
  border-radius: 0.25rem;
  margin-right: 0.5rem;
}

.search button {
  padding: 0.5rem 1rem;
  background-color: #555;
  color: #fff;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
}

.search button:hover {
  background-color: #777;
}

@media screen and (max-width: 768px) {
  .navbar {
    grid-template-columns: 1fr;
    grid-gap: 1rem;
    text-align: center;
  }

  .search {
    justify-content: center;
    margin-top: 1rem;
  }
}
</style>
