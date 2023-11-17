<template>
  <div>
    <MoviesCatalog :movies="movies" v-if="isDisplayCatalog" @select-movie="selectMovie"></MoviesCatalog>
    <MovieForm v-if="isDisplayForm" @save-movie="saveMovie" @delete-movie="deleteMovie"
               :movie="getSelectedMovie"></MovieForm>
    <button class="btn bottom-right-fixed-button" @click="handleButtonAddClick">{{ formAction }}</button>
    <button v-if="isDisplayForm" class="btn bottom-left-fixed-button" @click="handleButtonBackClick">Back</button>
  </div>
</template>

<script>
import uniqueId from "lodash.uniqueid";
import MoviesCatalog from "@/components/MoviesCatalog.vue";
import MovieForm from "@/components/MovieForm.vue";


export default {
  title: "app",
  components: {
    MovieForm,
    MoviesCatalog
  },
  data() {
    return {
      formAction: "add",
      currentPage: "catalog",
      selectedId: "",
      movies: [
        {
          id: uniqueId("movie-"),
          title: "The Shawshank Redemption",
          director: "Frank Darabont",
          genres: ["drama"],
          summary: "Over the course of several years, two convicts form a friendship, seeking consolation and, eventually, redemption through basic compassion."
        },
        {
          id: uniqueId("movie-"),
          title: "The Godfather",
          director: "Francis Ford Coppola",
          genres: ["crime", "drama"],
          summary: "Don Vito Corleone, head of a mafia family, decides to hand over his empire to his youngest son Michael. However, his decision unintentionally puts the lives of his loved ones in grave danger."
        },
        {
          id: uniqueId("movie-"),
          title: "The Dark Knight",
          director: "Christopher Nolan",
          genres: ["action", "crime", "drama"],
          summary: "When the menace known as the Joker wreaks havoc and chaos on the people of Gotham, Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice."
        },
        {
          id: uniqueId("movie-"),
          title: "Schindler's List",
          director: "Frank Darabont",
          genres: ["biography", "drama", "history"],
          summary: "In German-occupied Poland during World War II, industrialist Oskar Schindler gradually becomes concerned for his Jewish workforce after witnessing their persecution by the Nazis."
        },
        {
          id: uniqueId("movie-"),
          title: "Inception",
          director: "Christopher Nolan",
          genres: ["action", "adventure", "sci-fi"],
          summary: "A thief who steals corporate secrets through the use of dream-sharing technology is given the inverse task of planting an idea into the mind of a C.E.O., but his tragic past may doom the project and his team to disaster."
        },
        {
          id: uniqueId("movie-"),
          title: "The Lord of the Rings: The Two Towers",
          director: "Peter Jackson",
          genres: ["action", "adventure", "drama"],
          summary: "While Frodo and Sam edge closer to Mordor with the help of the shifty Gollum, the divided fellowship makes a stand against Sauron's new ally, Saruman, and his hordes of Isengard."
        }
      ]
    };
  },
  methods: {
    handleButtonAddClick() {
      this.currentPage = "form";
    },
    handleButtonBackClick() {
      console.log(this.selectedId);
      this.currentPage = "catalog";
    },
    saveMovie(movie) {
      if (movie.id === "") {
        movie.id = uniqueId("movie-");
      }

      this.movies = [...this.movies.filter(v => v.id !== movie.id), movie];
    },
    deleteMovie(id) {
      this.movies = this.movies.filter(v => v.id !== id);
      this.currentPage = "catalog";
    },
    selectMovie(id) {
      this.selectedId = id;
      this.formAction = id !== "" ? "Edit" : "Add";
    }
  },
  computed: {
    isDisplayCatalog() {
      return this.currentPage === "catalog";
    },
    isDisplayForm() {
      return this.currentPage === "form";
    },
    getSelectedMovie() {
      const filtered = this.movies.filter(v => v.id === this.selectedId);

      if (filtered.length === 0) {
        return null;
      }

      return filtered[0];
    }
  }
};
</script>

<style>
/* Global styles */
.btn {
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}

.btn__danger {
  color: #fff;
  background-color: #ca3c3c;
  border-color: #bd2130;
}

.btn__filter {
  border-color: lightgrey;
}

.btn__danger:focus {
  outline-color: #c82333;
}

.btn__primary {
  color: #fff;
  background-color: #31754f;
}

.btn-group {
  display: flex;
  justify-content: space-between;
}

.btn-group > * {
  flex: 1 1 auto;
}

.btn-group > * + * {
  margin-left: 0.8rem;
}

.label-wrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}

[class*="__lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}

[class*="__lg"]:not(:last-child) {
  margin-bottom: 1rem;
}

@media screen and (min-width: 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}

.visually-hidden {
  position: absolute;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  white-space: nowrap;
}

[class*="stack"] > * {
  margin-top: 0;
  margin-bottom: 0;
}

.stack-small > * + * {
  margin-top: 1.25rem;
}

.stack-large > * + * {
  margin-top: 2.5rem;
}

@media screen and (min-width: 550px) {
  .stack-small > * + * {
    margin-top: 1.4rem;
  }

  .stack-large > * + * {
    margin-top: 2.8rem;
  }
}

/* End global styles */
#app {
  background: #fff;
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  padding-top: 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
}

@media screen and (min-width: 550px) {
  #app {
    padding: 4rem;
  }
}

#app > * {
  max-width: 50rem;
  margin-left: auto;
  margin-right: auto;
}

#app > form {
  max-width: 100%;
}

#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}


.movie-title {
  font-weight: 700;
}

ul.genres-list {
  list-style-type: none;
  overflow: auto; /* Clear the float */
}

ul.genres-list li {
  float: left;
  display: block;
  background-color: #f0f0f0;
  padding: 10px;
  margin-right: 10px;
  text-transform: capitalize;
  margin-top: 10px;
  cursor: pointer;
}

ul.genres-list li.active {
  background-color: #204aaf;
  color: #f0f0f0;
}

.movie-item {
  cursor: pointer;
}

.movie-item.active {
  border-right: solid #3498db 5px;
}

.bottom-right-fixed-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  padding: 10px;
  background-color: #3498db;
  color: #fff;
  border: none;
  cursor: pointer;
}

.bottom-left-fixed-button {
  position: fixed;
  bottom: 20px;
  right: 100px;
  padding: 10px;
  background-color: #3498db;
  color: #fff;
  border: none;
  cursor: pointer;
}

.button-form-container {
  text-align: right;
}

.button-form-container button {
  margin-right: 10px;
}

.button-form-container button:last-child {
  margin-right: 0px;
}

#movie-form input, #movie-form textarea {
  width: 100%;
}

.form-message {
  color: #31754f;
  text-align: center;
}
</style>
