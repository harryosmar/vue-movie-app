<template>
  <div id="movie-form">
    <div class="button-form-container">
      <button v-if="id !== ''" class="btn btn__danger" id="btn-save-movie" @click="handleClickDelete">Delete</button>
      <button class="btn btn__primary" id="btn-delete-movie" @click="handleClickSave">Save</button>
    </div>
    <h2 class="form-message">{{message}}</h2>
    <ul>
      <li class="movie-title">
        <p>
          Title
        </p>
        <p>
          <input type="text" name="movie-title" v-model.trim="title">
        </p>
      </li>
      <li class="movie-director">
        <p>
          Director
        </p>
        <p>
          <input type="text" name="movie-director" v-model.trim="director">
        </p>
      </li>
      <li class="movie-summary">
        <p>
          Summary
        </p>
        <p>
          <textarea name="movie-summary" v-model.trim="summary"></textarea>
        </p>
      </li>
      <li>
        <ul class="genres-list">
          <li v-for="(genre, index) in genresOptions" :key="index" @click="() => handleClickGenre(genre)" :class="genres.lastIndexOf(genre) > -1 ? 'option-genre active': 'option-genre'">
            <label>{{ genre }}</label>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    movie: null,
  },
  methods: {
    handleClickGenre(genre) {
      const s = genre.toLowerCase();
      const index = this.genres.lastIndexOf(genre);
      if (index > -1) {
        this.genres = this.genres.filter(v => v !== s);
      } else {
        this.genres = [...this.genres, s];
      }
    },
    handleClickSave() {
      this.$emit("save-movie", {id: this.id, title: this.title, director: this.director, summary: this.summary, genres: this.genres,});
      this.id = '';
      this.title = '';
      this.director = '';
      this.summary = '';
      this.genres = [];
      this.message = "success save movie";
    },
    handleClickDelete() {
      this.$emit("delete-movie", this.id);
    },
  },
  data() {
    return {
      id: this.movie?.id || "",
      title: this.movie?.title || "",
      director: this.movie?.director|| "",
      genres: this.movie?.genres|| [],
      summary: this.movie?.summary|| "",
      genresOptions: ["action", "crime", "drama", "biography", "adventure", "history", "sci-fi"],
      message: "",
    };
  }
};
</script>
