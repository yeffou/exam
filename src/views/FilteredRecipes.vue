<template>
  <div class="filtered-recipes">
    <h1>Recettes Filtrées</h1>
    <div class="search-container">
      <input type="text" v-model="searchQuery" placeholder="Rechercher par nom..." class="search-input">
      <select v-model="selectedCategory" class="category-select">
        <option value="">Toutes les catégories</option>
        <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
      </select>
    </div>
    <div class="recipes-container">
      <div v-for="recipe in filteredRecipes" :key="recipe?.id" class="recipe-card">
        <h2>{{ recipe?.nom }}</h2>
        <img :src="recipe?.image" :alt="recipe?.nom" class="recipe-image" />
        <p class="recipe-description">{{ recipe?.description }}</p>
      </div>
    </div>
    <div v-if="error" class="error-message">{{ error }}</div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import { projectFirestore } from '@/firebase/Config.js';

const recipes = ref([]);
const error = ref(null);
const searchQuery = ref('');
const selectedCategory = ref('');

const loadRecipes = async () => {
  try {
    const res = await projectFirestore.collection('recipes').get();
    recipes.value = res.docs.map(doc => ({ ...doc.data(), id: doc.id }));
  } catch (err) {
    error.value = err.message;
  }
};

onMounted(() => {
  loadRecipes();
});

const filteredRecipes = computed(() => {
  let filtered = recipes.value;
  if (searchQuery.value.trim() !== '') {
    filtered = filtered.filter(recipe => recipe?.nom.toLowerCase().includes(searchQuery.value.toLowerCase()));
  }
  if (selectedCategory.value !== '') {
    filtered = filtered.filter(recipe => recipe?.category === selectedCategory.value);
  }
  return filtered;
});

const categories = computed(() => {
  const allCategories = [...new Set(recipes.value.map(recipe => recipe.category))];
  return allCategories.filter(category => category !== undefined && category !== '');
});
</script>

<style scoped>
.filtered-recipes {
  padding: 20px;
}

.search-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.search-input {
  padding: 10px;
  width: 60%;
}

.category-select {
  padding: 10px;
  width: 35%;
}

.recipes-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.recipe-card {
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s, box-shadow 0.3s;
}

.recipe-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.recipe-image {
  width: 100%;
  height: auto;
  object-fit: cover;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}

.recipe-description {
  padding: 15px;
  font-size: 1rem;
  color: #555;
  text-align: justify;
}

.error-message {
  color: red;
  text-align: center;
  margin-top: 20px;
}
</style>
