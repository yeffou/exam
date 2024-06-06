<template>
  <div class="category">
    <HeaderComp />
    <div class="category-header">
      <h1>{{ category }} Recipes</h1>
      <input type="text" v-model="searchQuery" placeholder="Search recipes..." class="search-input" />
    </div>
    <div class="recipes-container" v-if="recipes.length">
      <div v-for="recipe in recipes" :key="recipe.id" class="recipe-card">
        <div class="recipe-info">
          <h2>{{ recipe.name }}</h2>
          <p>{{ recipe.description }}</p>
        </div>
        <div class="recipe-actions">
          <router-link :to="{ name: 'RecipeDetailView', params: { id: recipe.id } }" class="details-link">Details</router-link>
          <router-link :to="{ name: 'EditRecipe', params: { id: recipe.id } }" class="edit-link">Edit</router-link>
          <button @click="deleteRecipe(recipe.id)" class="delete-button">
            <i class="fa fa-trash"></i> Delete
          </button>
        </div>
      </div>
    </div>
    <div v-else class="no-recipes">
      <p>No recipes found for this category.</p>
    </div>
    <FooterComp />
  </div>
</template>

<script>
import { ref, watch } from 'vue';
import { projectFirestore } from '@/firebase/Config';
import HeaderComp from '../components/HeaderComp.vue';
import FooterComp from '../components/FooterComp.vue';
import { useRoute } from 'vue-router';

export default {
  name: 'CategoryView',
  components: {
    HeaderComp,
    FooterComp,
  },
  setup() {
    const recipes = ref([]);
    const route = useRoute();
    const category = ref(route.params.category);
    const searchQuery = ref('');

    const fetchRecipes = async (category) => {
      try {
        recipes.value = [];
        const querySnapshot = await projectFirestore.collection('recipes').where('regime', '==', category).get();
        querySnapshot.forEach((doc) => {
          recipes.value.push({ id: doc.id, ...doc.data() });
        });
      } catch (error) {
        console.error('Error fetching recipes:', error);
      }
    };

    const deleteRecipe = async (id) => {
      try {
        await projectFirestore.collection('recipes').doc(id).delete();
        recipes.value = recipes.value.filter(recipe => recipe.id !== id);
      } catch (error) {
        console.error('Error deleting recipe:', error);
      }
    };

    watch(
      () => route.params.category,
      (newCategory) => {
        category.value = newCategory;
        fetchRecipes(newCategory);
      },
      { immediate: true }
    );

    return {
      recipes,
      category,
      searchQuery,
      deleteRecipe,
    };
  },
};
</script>

<style scoped>
.category {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.category-header {
  background-color: #ff9800;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.category-header h1 {
  font-size: 2.5rem;
  color: #ffffff;
  margin: 0;
}

.search-input {
  padding: 10px;
  border-radius: 5px;
  border: none;
  outline: none;
}

.recipes-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  padding: 20px;
}

.recipe-card {
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.recipe-info {
  flex: 1;
}

.recipe-actions {
  display: flex;
  gap: 10px;
}

.details-link,
.edit-link {
  padding: 8px 15px;
  border-radius: 5px;
  background-color: #ff9800;
  color: #ffffff;
  text-decoration: none;
}

.delete-button {
  background-color: #f44336;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  color: #ffffff;
  cursor: pointer;
}

.no-recipes {
  text-align: center;
  margin-top: 20px;
}
</style>
