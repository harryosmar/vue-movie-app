<template>
  <div>
    <ul>
      <li class="movie-title">{{ title }}</li>
      <li>Director : {{ director }}</li>
      <li>
        <ul class="genres-list">
          <li v-for="(genre, index) in genres" :key="index">
            {{ genre }}
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    id: { required: true, type: String },
    title: { required: true, type: String },
    director: { required: true, type: String },
    genres: { required: false, default: [], },
    summary: { required: true, type: String },
  },
  methods: {
    handleClickMovieItem(id) {
      this.selectedId = id;
    },
  },
  data() {
    return {

    };
  },
};
</script>
