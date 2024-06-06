<template>
  <div class="home">
    <HeaderComp />
    <div class="content">
      <div class="main-content">
        <div class="banner">
          <img src="https://www.example.com/banner-image.jpg" alt="Banner Image" />
          <h1>Discover and Share Delicious Recipes</h1>
        </div>
        <div class="featured-recipes">
          <h2>Featured Recipes</h2>
          <div class="recipe-card" v-for="recipe in featuredRecipes" :key="recipe.id">
            <img :src="recipe.image" :alt="recipe.name" />
            <h3>{{ recipe.name }}</h3>
            <p>{{ recipe.description }}</p>
          </div>
        </div>
      </div>
      <div class="categories">
        <h2>Categories</h2>
        <ul>
          <li v-for="category in categories" :key="category">
            <router-link :to="{ name: 'Category', params: { category: category } }">{{ category }}</router-link>
          </li>
        </ul>
      </div>
    </div>
    <FooterComp />
  </div>
</template>

<script>
import HeaderComp from '../components/HeaderComp.vue';
import FooterComp from '../components/FooterComp.vue';

export default {
  components: {
    HeaderComp,
    FooterComp,
  },
  data() {
    return {
      categories: [
        'Omnivore',
        'Végétarien',
        'Végétalien (Vegan)',
        'Pescétarien',
        'Flexitarien',
        'Fruitarien',
        'Crudivore',
        'Paleo',
        'Cétogène (Keto)',
        'Sans gluten'
      ],
      featuredRecipes: [
        {
          id: 1,
          name: 'Spaghetti Bolognese',
          description: 'A classic Italian pasta dish with rich, savory sauce.',
          image: 'https://www.example.com/spaghetti.jpg',
        },
        {
          id: 2,
          name: 'Vegan Buddha Bowl',
          description: 'A healthy and colorful bowl of fresh veggies and grains.',
          image: 'https://www.example.com/buddha-bowl.jpg',
        },
        {
          id: 3,
          name: 'Grilled Salmon',
          description: 'Perfectly grilled salmon with a side of vegetables.',
          image: 'https://www.example.com/grilled-salmon.jpg',
        },
      ],
    };
  },
};
</script>

<style scoped>
.home {
  padding: 20px;
  background-color: #2e2e2e;
  font-family: 'Helvetica Neue', sans-serif;
  color: #f0f0f0;
}

.content {
  display: flex;
  justify-content: space-between;
}

.main-content {
  flex: 0 0 70%;
  padding-right: 20px;
}

.banner {
  text-align: center;
  margin-top: 20px;
  background-color: #3e3e3e;
  border-radius: 12px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.2);
  padding: 50px 30px;
  transition: transform 0.3s ease-in-out;
}

.banner:hover {
  transform: scale(1.05);
}

.banner img {
  max-width: 100%;
  border-radius: 12px;
  transition: transform 0.3s ease-in-out;
}

.banner h1 {
  font-size: 2.8rem;
  margin-top: 20px;
  color: #f0f0f0;
  font-weight: bold;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
}

.featured-recipes {
  margin: 40px 0;
  text-align: center;
}

.featured-recipes h2 {
  font-size: 2.4rem;
  margin-bottom: 25px;
  color: #0800ff;
  text-shadow: 2px 2px 3px rgba(0, 0, 0, 0.2);
}

.recipe-card {
  display: inline-block;
  background-color: #3e3e3e;
  border-radius: 12px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.2);
  margin: 15px;
  padding: 20px;
  width: calc(33% - 30px);
  text-align: left;
}

.recipe-card img {
  max-width: 100%;
  border-radius: 8px;
  margin-bottom: 15px;
}

.recipe-card h3 {
  font-size: 1.7rem;
  margin-bottom: 10px;
  color: #0800ff;
}

.recipe-card p {
  font-size: 1.1rem;
  color: #e0e0e0;
}

.categories {
  flex: 0 0 30%;
  padding-left: 20px;
  text-align: center;
  background-color: #3e3e3e;
  border-radius: 12px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.2);
  padding: 30px;
}

.categories h2 {
  font-size: 2.4rem;
  margin-bottom: 25px;
  color: #0800ff;
  text-shadow: 2px 2px 3px rgba(0, 0, 0, 0.2);
}

.categories ul {
  list-style: none;
  padding: 0;
}

.categories li {
  margin-bottom: 15px;
}

.categories li a {
  display: block;
  padding: 12px 18px;
  background-color: #0800ff;
  color: #2e2e2e;
  font-size: 1.2rem;
  border-radius: 8px;
  text-decoration: none;
  transition: background-color 0.3s ease-in-out, color 0.3s ease-in-out;
}

.categories li a:hover {
  background-color: #0800ff;
  color: #f0f0f0;
}
</style>
