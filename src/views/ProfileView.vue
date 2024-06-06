<template>
  <div class="profile-view">
    <HeaderComp />
    <div class="profile-container">
      <div class="user-info" v-if="user">
        <h1>Profil de l'Utilisateur</h1>
        <p><strong>Nom:</strong> {{ user.displayName }}</p>
        <p><strong>Email:</strong> {{ user.email }}</p>
      </div>
      <div v-else>
        <p>Chargement des informations de l'utilisateur...</p>
      </div>
      <div class="user-recipes">
        <h2>Mes Recettes</h2>
        <div v-if="userRecipes.length">
          <div v-for="recipe in userRecipes" :key="recipe.id" class="recipe-card">
            <h3>{{ recipe.nom }}</h3>
            <img :src="recipe.image" :alt="recipe.nom" class="recipe-image" />
            <router-link :to="{ name: 'RecipeDetail', params: { id: recipe.id } }" class="details-link">Détails</router-link>
          </div>
        </div>
        <div v-else>
          <p>Vous n'avez ajouté aucune recette.</p>
        </div>
        <div v-if="error" class="error-message">{{ error }}</div>
      </div>
    </div>
    <FooterComp />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { projectFirestore, projectAuth } from '../firebase/Config.js';
import HeaderComp from '../components/HeaderComp.vue';
import FooterComp from '../components/FooterComp.vue';

export default {
  name: 'ProfileView',
  components: {
    HeaderComp,
    FooterComp,
  },
  setup() {
    const user = ref(null);
    const userRecipes = ref([]);
    const error = ref(null);

    onMounted(async () => {
      user.value = projectAuth.currentUser;
      if (user.value) {
        try {
          const res = await projectFirestore.collection('recipes')
            .where('userId', '==', user.value.uid)
            .get();

          userRecipes.value = res.docs.map(doc => ({ ...doc.data(), id: doc.id }));
        } catch (err) {
          error.value = err.message;
        }
      } else {
        error.value = "Utilisateur non connecté.";
      }
    });

    return { user, userRecipes, error };
  },
};
</script>

<style scoped>
.profile-view {
  display: flex;
  flex-direction: column;
  background-color: #1e1e1e;
  min-height: 100vh;
  color: #f0f0f0;
  font-family: 'Roboto', sans-serif;
}

.profile-container {
  padding: 30px;
  max-width: 900px;
  margin: 0 auto;
}

.user-info, .user-recipes {
  background: #333;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 6px 24px rgba(0, 0, 0, 0.3);
  margin-bottom: 25px;
}

.user-info h1, .user-recipes h2 {
  font-size: 2.2rem;
  color: #0800e9;
  margin-bottom: 25px;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
}

.user-info p {
  font-size: 1.2rem;
  color: #e0e0e0;
}

.recipe-card {
  display: flex;
  flex-direction: column;
  background: #444;
  border-radius: 12px;
  box-shadow: 0 6px 24px rgba(0, 0, 0, 0.3);
  margin-bottom: 25px;
  overflow: hidden;
  transition: transform 0.3s, box-shadow 0.3s;
}

.recipe-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 28px rgba(0, 0, 0, 0.35);
}

.recipe-card h3 {
  font-size: 1.6rem;
  margin: 0;
  padding: 15px;
  color: #0800e9;
  background-color: #222;
  text-align: center;
  border-bottom: 2px solid #0800e9;
}

.recipe-image {
  width: 100%;
  height: auto;
  object-fit: cover;
}

.details-link {
  display: block;
  text-align: center;
  padding: 12px 0;
  margin: 15px 20px;
  border-radius: 8px;
  background-color: #0800e9;
  color: #1e1e1e;
  text-decoration: none;
  font-size: 1.1rem;
  transition: background-color 0.3s, transform 0.3s;
}

.details-link:hover {
  background-color: #0800e9;
  transform: translateY(-3px);
}

.error-message {
  color: #ff4d4d;
  text-align: center;
  margin: 25px;
}
</style>
