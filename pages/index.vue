<template>
  <div class="home">
    <!-- Hero Component -->
    <Hero />
    <!-- Search -->
    <div class="container search">
      <input type="text" placeholder="Search movie and press enter" v-model.lazy="searchInput" />
      <button class="button" v-show="searchInput" @click="clear">
        Clear Search
      </button>
    </div>
    <p class="container error" style="color: #fff" v-if="!movies.length">
      Error while fetching mountains
    </p>
    <!-- Loading -->
    <Loading v-if="loading && !error" />

    <!-- Movies Component -->
    <Movies :movies="movies" v-if="!error" />

    <!-- Pagination -->
    <div class="container pagination" v-if="movies.length >= 20">
      <button class="button" @click="prevPage" v-if="totalPages > 1">
        Previous Page
      </button>
      <button class="button" @click="nextPage" >Next Page</button>
    </div>

    <!-- Footer -->
    <div class="container footer">
      <p>
        Made with <span class="heart">❤</span> by
        <a href="https://github.com/matiuxdev" target="_blank">MatiuxDev</a>
      </p>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Home',
  data() {
    return {
      movies: [],
      searchInput: '',
      loading: false,
      error: false,
      totalPages: 1,
    }
  },
  async fetch() {
    if (!this.searchInput) {
      await this.getMovies()
      return
    }
    await this.searchMovies()
  },
  methods: {
    async getMovies(page = 1) {
      this.loading = true
      this.movies = []
      const { results } = await this.$axios
        .$get(
          `movie/now_playing?api_key=${process.env.API_KEY}&language=en-US&page=${page}`
        )
        .catch((err) => {
          this.error = true
          console.log(err.response.data.status_message)
          this.loading = false
        })
      this.movies = results
      this.loading = false
    },
    async searchMovies(page = 1) {
      this.loading = true
      const { results } = await this.$axios
        .$get(
          `search/movie?api_key=${process.env.API_KEY}&language=en-US&query=${this.searchInput}&page=${page}&include_adult=false`
        )
        .catch((err) => {
          this.error = true
          console.log(err.response.data.status_message)
          this.loading = false
        })
      this.movies = results
      this.loading = false
      console.log(this.movies.length)
    },
    clear() {
      this.searchInput = ''
      this.totalPages = 1;
    },
    async nextPage() {
      this.loading = true
      const page = ++this.totalPages
      this.searchInput
        ? await this.searchMovies(page)
        : await this.getMovies(page)
    },
    async prevPage() {
      this.loading = true
      const page = --this.totalPages
      this.searchInput
        ? await this.searchMovies(page)
        : await this.getMovies(page)
    },
  },
  watch: {
    async searchInput() {
      if (this.searchInput) {
        await this.searchMovies()
      } else {
        await this.getMovies()
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 2rem;
}
.pagination button {
  margin: 0 1rem;
}
.footer {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 2rem;

}
.footer p {
  color: #fff;
  margin: 0;
}
.footer p a {
  color: #f00;
  &:hover {
    color: #fff;
    text-decoration: underline;
  }
}
.heart {
  color: #f00;
}
.error {
  color: #f00;
  font-size: 1.5rem;
  text-align: center;
}
</style>
