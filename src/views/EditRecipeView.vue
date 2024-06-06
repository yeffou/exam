<template>
  <div class="edit-recipe">
    <HeaderComp />
    <main class="container py-5">
      <div class="card p-4 shadow-lg border-0">
        <form @submit.prevent="editRecipe">
          <div class="form-group row mb-3">
            <label for="name" class="col-sm-3 col-form-label">Nom de la Recette</label>
            <div class="col-sm-9">
              <input type="text" id="name" class="form-control" v-model="name" required />
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="image" class="col-sm-3 col-form-label">URL de l'Image</label>
            <div class="col-sm-9">
              <input type="url" id="image" class="form-control" v-model="image" required />
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="description" class="col-sm-3 col-form-label">Description</label>
            <div class="col-sm-9">
              <textarea id="description" class="form-control" v-model="description" rows="3" required></textarea>
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="ingredients" class="col-sm-3 col-form-label">Ingrédients</label>
            <div class="col-sm-9">
              <textarea id="ingredients" class="form-control" v-model="ingredients" rows="3" required></textarea>
              <small class="form-text text-muted">Un ingrédient par ligne</small>
            </div>
          </div>
          <div class="form-group row mb-4">
            <label for="instructions" class="col-sm-3 col-form-label">Instructions</label>
            <div class="col-sm-9">
              <textarea id="instructions" class="form-control" v-model="instructions" rows="5" required></textarea>
            </div>
          </div>
          <button type="submit" class="btn btn-warning w-100 text-dark fw-bold">Modifier la Recette</button>
        </form>
      </div>
    </main>
    <FooterComp />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { projectFirestore } from '@/firebase/Config';
import { useRoute } from 'vue-router';
import HeaderComp from '@/components/HeaderComp.vue';
import FooterComp from '@/components/FooterComp.vue';

const route = useRoute();
const recipeId = route.params.id;

const name = ref('');
const image = ref('');
const description = ref('');
const ingredients = ref('');
const instructions = ref('');

const fetchRecipe = async () => {
  try {
    const recipeDoc = await projectFirestore.collection('recipes').doc(recipeId).get();
    if (recipeDoc.exists) {
      const recipeData = recipeDoc.data();
      name.value = recipeData.name;
      image.value = recipeData.image;
      description.value = recipeData.description;
      ingredients.value = recipeData.ingredients.join('\n');
      instructions.value = recipeData.instructions;
    }
  } catch (error) {
    console.error('Error fetching recipe:', error);
  }
};

onMounted(() => {
  fetchRecipe();
});

const editRecipe = async () => {
  try {
    await projectFirestore.collection('recipes').doc(recipeId).update({
      name: name.value,
      image: image.value,
      description: description.value,
      ingredients: ingredients.value.split('\n').map(ingredient => ingredient.trim()),
      instructions: instructions.value,
      updatedAt: new Date()
    });
    alert('Recette modifiée avec succès!');
  } catch (error) {
    console.error('Error editing recipe:', error);
    alert('Erreur lors de la modification de la recette.');
  }
};
</script>

<style scoped>
.edit-recipe {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #f9f9f9;
}

.card {
  background-color: #ffffff;
  border-radius: 10px;
  padding: 2rem;
  max-width: 800px;
  margin: 0 auto;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 2rem;
}

.form-group label {
  font-weight: bold;
}

.form-control {
  border-radius: 5px;
}

.btn {
  border-radius: 5px;
  font-weight: bold;
}

@media (max-width: 576px) {
  .card {
    padding: 1.5rem;
  }
}
</style>
