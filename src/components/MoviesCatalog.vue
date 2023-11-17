<template>
  <div>
    <h1>Movies Collection</h1>
    <MovieSearchBar @set-query="setQuery"></MovieSearchBar>
    <h2 id="list-summary" ref="listSummary" tabindex="-1">{{ filteredMovies.length }} movies available</h2>
    <ul aria-labelledby="list-summary" class="stack-large">
      <MovieItem v-for="(movie, index) in filteredMovies" :key="index" v-bind="movie"
                 @click="() => handleClickMovieItem(movie.id)"
                 :class="movie.id === selectedId ? 'movie-item active': 'movie-item'"></MovieItem>
    </ul>
  </div>
</template>

<script>
import MovieSearchBar from "@/components/MovieSearchBar.vue";
import MovieItem from "@/components/MovieItem.vue";

export default {
  components: {
    MovieItem,
    MovieSearchBar
  },
  data() {
    return {
      query: "",
      selectedId: ""
    };
  },
  props: {
    movies: { required: false, default: [] }
  },
  methods: {
    setQuery(query) {
      this.query = query;
    },
    handleClickMovieItem(id) {
      if (this.selectedId === id) {
        this.selectedId = "";
      } else {
        this.selectedId = id;
      }
      this.$emit("select-movie", this.selectedId);
    }
  },
  computed: {
    filteredMovies() {
      if (!this.query) {
        return [...this.movies];
      }
      const regex = new RegExp(this.query, "i");
      return this.movies.filter(v => !!v.title.match(regex));
    }
  }
};
</script>
