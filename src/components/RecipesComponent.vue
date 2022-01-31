<template>
  <div class="container">
    <div class="row mb-3">
      <input
        type="text"
        v-model="query"
        class="form-control"
        @keypress="fetchRecipes"
      />
    </div>
    <div class="row">
      <div v-for="(row, key, index) in recipes" :key="row.id">
        <div class="card" style="width: 18rem">
          <img class="card-img-top img-custom-height" v-bind:src="row.strMealThumb?row.strMealThumb : '../assets/img-placeholder.png'" alt="Card image cap" />
          <div class="card-body">
            <h5 class="card-title">{{row.strMeal}}</h5>
            <p class="card-text">
             {{row.strInstructions}}
            </p>
            <a v-bind:href=row.strSource class="btn btn-primary">Show more</a>
          </div>
        </div>
      </div>
      <div v-if="recipes==null" class="alert alert-warning">
        <p>Not found</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Recipes",
  data() {
    return {
      query: "",
      recipes: {},
    };
  },
  methods: {
    fetchRecipes(e) {
      if (e.key == "Enter") {
        fetch(
          "https://www.themealdb.com/api/json/v1/1/search.php?s=" + this.query,
          {
            method: "GET",
            header: { "Content-Type": "application/json" },
          }
        )
          .then((response) => {
            return response.json();
          })
          .then(this.setMeals);
      }
    },
    setMeals(meals) {
      this.recipes = meals.meals;
      console.log(this.recipes);
    },
    getDataHome() {
        fetch(
          "https://www.themealdb.com/api/json/v1/1/search.php?s=Soup" ,
          {
            method: "GET",
            header: { "Content-Type": "application/json" },
          }
        )
          .then((response) => {
            return response.json();
          })
          .then(this.setMeals);
    }
  },
  mounted() {
    this.getDataHome();
  }
};
</script>
<style>
.img-custom-height {
  height: 200px !important;
  width: auto !important;
}
.card-text {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;  
  overflow: hidden;

}
.row > div {
  width: auto;
  margin-bottom: 30px;
}
</style>
