<template>
  <div class="recipe-detail">
    <HeaderComp />
    <div class="container py-5">
      <div v-if="recipe">
        <h1>{{ recipe.name }}</h1>
        <img :src="recipe.image" alt="Recipe Image" class="img-fluid rounded mb-3" />
        <p>{{ recipe.description }}</p>
        <h3>Ingrédients</h3>
        <ul>
          <li v-for="ingredient in recipe.ingredients" :key="ingredient">{{ ingredient }}</li>
        </ul>
        <h3>Instructions</h3>
        <p>{{ recipe.instructions }}</p>
      </div>
      <div v-else>
        <p>Chargement...</p>
      </div>
    </div>
    <FooterComp />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { projectFirestore } from '@/firebase/Config';
import { useRoute } from 'vue-router';
import HeaderComp from '../components/HeaderComp.vue';
import FooterComp from '../components/FooterComp.vue';

export default {
  name: 'RecipeDetailView',
  components: {
    HeaderComp,
    FooterComp,
  },
  setup() {
    const recipe = ref(null);
    const route = useRoute();

    onMounted(async () => {
      const doc = await projectFirestore.collection('recipes').doc(route.params.id).get();
      if (doc.exists) {
        recipe.value = doc.data();
      }
    });

    return {
      recipe,
    };
  },
};
</script>

<style scoped>
.recipe-detail {
  padding: 20px;
  background-color: #f3f4f6;
  font-family: 'Roboto', sans-serif;
}

.recipe-detail img {
  max-width: 100%;
  border-radius: 12px;
  margin-bottom: 20px;
}

.recipe-detail h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
  color: #0800e9;
}

.recipe-detail ul {
  list-style: none;
  padding: 0;
}

.recipe-detail p {
  font-size: 1.2rem;
  color: #333;
  line-height: 1.6;
}

.recipe-detail h3 {
  font-size: 1.8rem;
  margin-top: 20px;
  margin-bottom: 10px;
  color: #0800e9;
}



.recipe-detail ul li {
  background-color: #e0e0ff;
  padding: 10px;
  border-radius: 5px;
  margin-bottom: 5px;
  color: #333;
}
</style>
