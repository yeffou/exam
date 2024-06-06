<template>
  <div class="add-recipe" :class="'bg-' + currentBackgroundIndex">
    <HeaderComp />
    <main class="container py-5">
      <div class="card p-4 shadow-lg border-0">
        <form @submit.prevent="addRecipe">
          <div class="form-group row mb-3">
            <label for="name" class="col-sm-3 col-form-label">Nom de la Recette</label>
            <div class="col-sm-9">
              <input type="text" id="name" class="form-control" v-model="name" required />
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="image" class="col-sm-3 col-form-label">URL de l'Image</label>
            <div class="col-sm-9">
              <input type="text" id="image" class="form-control" v-model="image" required />
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="description" class="col-sm-3 col-form-label">Description</label>
            <div class="col-sm-9">
              <textarea id="description" class="form-control" v-model="description" rows="3" required></textarea>
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="category" class="col-sm-3 col-form-label">Catégorie</label>
            <div class="col-sm-9">
              <select id="category" class="form-control" v-model="category" required>
                <option value="" disabled>Choisir une catégorie</option>
                <option value="Omnivore">Omnivore</option>
                <option value="Végétarien">Végétarien</option>
                <option value="Végétalien (Vegan)">Végétalien (Vegan)</option>
                <option value="Pescétarien">Pescétarien</option>
                <option value="Flexitarien">Flexitarien</option>
                <option value="Fruitarien">Fruitarien</option>
                <option value="Crudivore">Crudivore</option>
                <option value="Paleo">Paleo</option>
                <option value="Cétogène (Keto)">Cétogène (Keto)</option>
                <option value="Sans gluten">Sans gluten</option>
              </select>
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="ingredients" class="col-sm-3 col-form-label">Ingrédients</label>
            <div class="col-sm-9">
              <input type="text" id="ingredients" class="form-control" v-model="ingredients" required />
              <small class="form-text text-muted">Séparés par une virgule</small>
            </div>
          </div>
          <div class="form-group row mb-4">
            <label for="instructions" class="col-sm-3 col-form-label">Instructions</label>
            <div class="col-sm-9">
              <textarea id="instructions" class="form-control" v-model="instructions" rows="5" required></textarea>
            </div>
          </div>
          <button type="submit" class="btn btn-warning w-100 text-dark fw-bold">Ajouter la Recette</button>
        </form>
      </div>
    </main>
    <footer class="bg-orange text-white py-3 mt-auto">
      <div class="container text-center">
        <p>&copy; 2024 DietaryApp. Tous droits réservés.</p>
      </div>
    </footer>
  </div>
</template>

<script>
import { ref  } from 'vue';
import { projectFirestore } from '@/firebase/Config';
import HeaderComp from '../components/HeaderComp.vue';

export default {
  name: 'AddRecipeView',
  components: {
    HeaderComp,
  },
  setup() {
    const name = ref('');
    const image = ref('');
    const description = ref('');
    const ingredients = ref('');
    const instructions = ref('');
    const category = ref('');

    

    


    const addRecipe = async () => {
      try {
        await projectFirestore.collection('recipes').add({
          name: name.value,
          image: image.value,
          description: description.value,
          category: category.value,
          ingredients: ingredients.value.split(',').map(ingredient => ingredient.trim()),
          instructions: instructions.value,
          popularity: 0,
          createdAt: new Date()
        });
        alert('Recette ajoutée avec succès!');
        name.value = '';
        image.value = '';
        description.value = '';
        category.value = '';
        ingredients.value = '';
        instructions.value = '';
      } catch (error) {
        console.error('Error adding recipe:', error);
        alert('Erreur lors de l\'ajout de la recette.');
      }
    };

    return {
      name,
      image,
      description,
      category,
      ingredients,
      instructions,
      addRecipe,
      
    };
  }
}
</script>

<style scoped>
.add-recipe {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-size: cover;
  background-position: center;
  transition: background-image 1s ease-in-out;
}




.bg-orange {
  background-color: #ff9800 !important;
}

header {
  text-align: center;
}

.card {
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 10px;

}


</style>