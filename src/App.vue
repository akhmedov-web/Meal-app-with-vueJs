<script>
import "../node_modules/bootstrap/dist/css/bootstrap.min.css";
import "../node_modules/bootstrap/dist/js/bootstrap.bundle.js"
import axios from "axios"
import Footer from "./components/Footer.vue";
import Error from "./components/Error.vue";
import Loading from "./components/Loading.vue";
export default {
  name: "app",
  components: { Footer, Error, Loading },
  data() {
    return {
      data: [],
      value: "",
      loading: true,
      mode: false,
      darkMode: {
        backgroundColor: '#1D2225',
        color: '#fff'
      },
      lightMode: {
        backgroundColor: '#fff'
      }
    }
  },
  async created() {
    try {
      const response = await axios.get(`https://www.themealdb.com/api/json/v1/1/filter.php?c=Seafood`);
      this.data = response.data.meals;
      console.log(response.data.meals)
      console.log(this.mode)
    } catch (e) {
      console.log(e);
    }
  },
  methods: {
    async getData() {
      try {
        const res = await axios.get(`https://www.themealdb.com/api/json/v1/1/search.php?s=${this.value}`);
        this.data = res.data.meals;
        console.log(res.data.meals)
      } catch (e) {
        console.log(e);
      }
    },
    getLoading: function () { setTimeout(() => { this.loading = false }, 1000) },
    toogle: function () {
      this.mode = !this.mode
    }
  },
  mounted() {
    this.getLoading()
  }
}
</script>
<template>
  <div>
    <nav class="navbar navbar-expand-lg">
      <div class="container-fluid d-flex">
        <a class="navbar-brand text-light" href="#">Meal app</a>
        <button class="navbar-toggler text-light" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="bi bi-search"></span>
        </button>
        <div class="form-check form-switch switch">
            <input class="form-check-input" @change="toogle" type="checkbox" role="switch"
              id="flexSwitchCheckDefault" />
          </div>
        <div class="collapse navbar-collapse justify-content-end" id="navbarSupportedContent">
          <div class="d-flex">
            <input type="text" :class="mode ? 'form-control me-2 bg-dark text-light' : 'form-control me-2'"
              @change="(e) => this.value = e.target.value" placeholder="Search meal by name" />
            <button :class="mode ? 'btn btn-outline-light text-light bg-dark' : 'btn btn-outline-light text-light'"
              @click="getData()">Search</button>
          </div>
        </div>
      </div>
    </nav>
    <!-- main -->
    <div class="main pt-4" v-if="data !== null" :style="mode ? darkMode : lightMode">
      <div class="d-flex flex-xl-row flex-column w-100 justify-content-between">
        <nav aria-label="breadcrumb">
          <ol class="breadcrumb ms-xl-5 ms-2">
            <li class="breadcrumb-item h5"><a href="">Home</a></li>
            <li class="breadcrumb-item active h5" aria-current="page">{{ this.value }}</li>
          </ol>
        </nav>
        <h4 class="mb-4 me-xl-4 me-0 ms-xl-0 ms-2">Recommended <span class="badge bg-warning">for you</span></h4>
      </div>
      <Loading v-if="this.loading" />
      <div
        class=" w-100 d-flex flex-xl-row flex-column justify-content-around align-items-xl-start align-items-center flex-wrap"
        v-else>
        <div class="mealCard mb-5" v-for="item of data">
          <img class="cardImg" v-bind:src="item.strMealThumb" />
          <div class="card-title text-xl-end text-start py-2">
            <h5>{{ item.strMeal }}</h5>
          </div>
        </div>
      </div>
    </div>
    <Error v-else />
    <!-- footer  -->
    <Footer />
  </div>
</template>
<style scoped>
</style>