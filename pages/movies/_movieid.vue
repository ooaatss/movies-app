<template>
  <!-- Loading -->
  <Loading v-if="$fetchState.pending" />

  <!-- Movie Info -->
  <div v-else class="single-movie container">
    <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
        <img
          :src="
            movie.poster_path
              ? `https://image.tmdb.org/t/p/w500/${movie.poster_path}`
              : noImage
          "
          alt=""
        />
        <p class="review">{{ movie.vote_average }}</p>
      </div>
      <div class="movie-content">
        <h1>Title: {{ movie.title }}</h1>
        <p class="movie-fact tagline">
          <span>Tagline:</span> "{{
            movie.tagline ? movie.tagline : 'No available'
          }}"
        </p>
        <p class="movie-fact">
          <span>Released:</span>
          {{
            new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Duration:</span> {{ movie.runtime }} minutes
        </p>
        <p class="movie-fact">
          <span>Revenue:</span>
          {{
            movie.revenue.toLocaleString('en-us', {
              style: 'currency',
              currency: 'USD',
            })
          }}
        </p>
        <p class="movie-fact"><span>Overview:</span> {{ movie.overview }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'singleMovie',
  async fetch() {
    await this.getSingleMovie()
  },
  // delay for fetching
  fetchDelay: 1000,
  head() {
    return {
      title: this.movie.title,
    }
  },
  data() {
    return {
      movie: '',
      noImage:
        'https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/No-Image-Placeholder.svg/1200px-No-Image-Placeholder.svg.png',
    }
  },
  methods: {
    async getSingleMovie() {
      const movieId = this.$route.params.movieid
      const result = await this.$axios.$get(
        `https://api.themoviedb.org/3/movie/${movieId}?api_key=${process.env.API_KEY}&language=en-US`
      )
      this.movie = result
      console.log(result)
    },
  },
}
</script>

<style lang="scss">
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      position: relative;
      img {
        max-height: 500px;
        width: 100%;
        border-radius: 20px;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
      .review {
        position: absolute;
        top: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 80px;
        height: 80px;
        background-color: #c92502;
        font-size: 25px;
        color: #fff;
        border-radius: 16px 0;
        box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
          0 2px 4px -1px rgba(0, 0, 0, 0.06);
        @media (max-width: 800px) {
          width: 40px;
          height: 40px;
          font-size: 16px;
        }
      }
      .overview {
        line-height: 1.5;
        position: absolute;
        bottom: 0;
        background-color: rgba(201, 38, 2, 0.9);
        padding: 12px;
        color: #fff;
        border-radius: 0 0 16px 16px;
        transform: translateY(100%);
        transition: 0.3s ease-in-out all;
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
        @media (min-width: 800px) {
          font-size: 40px;
        }
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>