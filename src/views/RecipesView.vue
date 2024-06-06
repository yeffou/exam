<template>
  <div class="recipes-view">
    <HeaderComp />
    <div class="container py-5">
      <div v-for="recipe in recipes" :key="recipe.id" class="recipe-card">
        <div class="card-header">
          <h2 class="recipe-title">{{ recipe.nom }}</h2>
          <div class="actions">
            <router-link :to="{ name: 'RecipeDetailView', params: { id: recipe.id } }" class="details-link">Details</router-link>
            <router-link :to="{ name: 'EditRecipe', params: { id: recipe.id } }" class="edit-link">Edit</router-link>
            <button @click="deleteRecipe(recipe.id)" class="delete-button">
              <i class="fa fa-trash"></i> Delete
            </button>
          </div>
        </div>
        <img :src="recipe.image" :alt="recipe.nom" class="recipe-image" />
        <p class="recipe-description">{{ recipe.description }}</p>
      </div>
    </div>
    <FooterComp />
  </div>
</template>

<script>
import { projectFirestore } from '../firebase/Config.js';
import { ref, onMounted } from 'vue';
import HeaderComp from '../components/HeaderComp.vue';
import FooterComp from '../components/FooterComp.vue';

export default {
  name: 'RecipesView',
  components: {
    HeaderComp,
    FooterComp,
  },
  setup() {
    const recipes = ref([]);
    const error = ref(null);

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

    const deleteRecipe = async (id) => {
      try {
        await projectFirestore.collection('recipes').doc(id).delete();
        console.log("Document successfully deleted!");
        recipes.value = recipes.value.filter(recipe => recipe.id !== id);
      } catch (error) {
        console.error("Error removing document: ", error);
      }
    };

    return { recipes, error, deleteRecipe };
  },
};
</script>

<style scoped>
.recipes-view {
  background-color: #f9f9f9;
  min-height: 100vh;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.recipe-card {
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s, box-shadow 0.3s;
  margin-bottom: 20px;
}

.recipe-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background-color: #0800e9;
  color: #fff;
}

.actions {
  display: flex;
}

.actions > * {
  margin-left: 10px;
}

.recipe-title {
  font-size: 1.8rem;
  margin: 0;
}

.recipe-image {
  width: 100%;
  height: auto;
  object-fit: cover;
}

.recipe-description {
  padding: 15px;
  font-size: 1.1rem;
  color: #333;
  text-align: justify;
}

.details-link,
.edit-link,
.delete-button {
  display: block;
  text-align: center;
  padding: 10px 0;
  border-radius: 5px;
  text-decoration: none;
  transition: background-color 0.3s;
  color: #fff;
}

.details-link {
  background-color: #4caf50;
}

.edit-link {
  background-color: #2196f3;
}

.delete-button {
  background-color: #f44336;
}

.details-link:hover,
.edit-link:hover,
.delete-button:hover {
  background-color: #0a8024;
}

.error-message {
  color: red;
  text-align: center;
  margin: 20px;
}
</style>
