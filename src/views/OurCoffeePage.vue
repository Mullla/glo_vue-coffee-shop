<template>
  <main>
    <div class="banner coffepage-banner">
      <div class="container">
        <div class="row">
          <div class="col-lg-6">
            <app-navbar />
          </div>
        </div>
        <h1 class="title-big">Our Coffee</h1>
      </div>
    </div>
    <section class="shop">
      <div class="container">
        <div class="row">
          <div class="col-lg-4 offset-2">
            <img
              class="shop__girl"
              src="@/assets/img/coffee_girl.jpg"
              alt="girl"
            />
          </div>
          <div class="col-lg-4">
            <div class="title">About our beans</div>
            <img
              class="beanslogo"
              src="@/assets/logo/Beans_logo_dark.svg"
              alt="Beans logo"
            />
            <div class="shop__text">
              Extremity sweetness difficult behaviour he of. On disposal of as
              landlord horrible.
              <br /><br />
              Afraid at highly months do things on at. Situation recommend
              objection do intention<br />
              so questions. <br />
              As greatly removed calling pleased improve an. Last ask him cold
              feel<br />
              met spot shy want. Children me laughing we prospect answered
              followed. At it went<br />
              is song that held help face.
            </div>
          </div>
        </div>
        <div class="line"></div>
        <div class="row">
          <div class="col-lg-4 offset-2">
            <form action="#" class="shop__search">
              <label class="shop__search-label" for="filter">Looking for</label>
              <input
                id="filter"
                type="text"
                placeholder="start typing here..."
                class="shop__search-input"
                @input="onSearch"
              />
            </form>
          </div>
          <div class="col-lg-4">
            <div class="shop__filter">
              <div class="shop__filter-label" @click="resetFilter">
                Or filter
              </div>
              <div class="shop__filter-group">
                <button class="shop__filter-btn" @click="onSort('Brazil')">
                  Brazil
                </button>
                <button class="shop__filter-btn" @click="onSort('Kenya')">
                  Kenya
                </button>
                <button class="shop__filter-btn" @click="onSort('Columbia')">
                  Columbia
                </button>
              </div>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-lg-10 offset-lg-1">
            <div class="shop__wrapper">
              <product-card
                v-for="coffee in goods"
                :key="coffee.id"
                :coffee="coffee"
                @onNavigate="navigate"
              />
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import AppNavbar from "@/components/AppNavbar";
import ProductCard from "@/components/ProductCard";
import { navigate } from "@/mixins/navigate";
import { debounce } from "debounce";

export default {
  components: {
    AppNavbar,
    ProductCard,
  },

  mixins: [navigate],

  data() {
    return {
      name: "CoffeePage",
    };
  },

  computed: {
    goods() {
      return this.$store.getters["getCoffee"];
    },
    searchValue: {
      set(value) {
        this.$store.dispatch("setSearchValue", value);
      },
      get() {
        return this.$store.state.coffee.searchValue;
      },
    },
  },

  mounted() {
    fetch("http://localhost:3000/coffee")
      .then((resp) => resp.json())
      .then((data) => {
        this.$store.dispatch("setCoffeeData", data);
      });
  },

  methods: {
    onSearch: debounce(function (event) {
      this.onSort(event.target.value);
    }, 500),
    onSearch(e) {
      this.onSort(e.target.value);
    },
    onSort(value) {
      fetch(`http://localhost:3000/coffee?q=${value}`)
        .then((resp) => resp.json())
        .then((data) => {
          this.$store.dispatch("setCoffeeData", data);
        });
    },
    resetFilter() {
      this.onSort("");
    },
  },
};
</script>