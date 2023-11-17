<template>
  <input
    type="text"
    id="search-input"
    name="search-movies"
    autocomplete="off"
    v-model.trim="query"
    @keyup.prevent="onSearch"
    class="input__lg" placeholder="Search by title..." />
</template>

<script>
export default {
  methods: {
    onSearch() {
      this.$emit("set-query", this.query);
    }
  },
  data() {
    return {
      query: ""
    };
  }
};
</script>
