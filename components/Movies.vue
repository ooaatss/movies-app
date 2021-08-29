<template>
  <div class="container movies">
    <div id="movie-grid" class="movies-grid">
      <div
        v-for="(
          { poster_path, vote_average, overview, title, release_date, id },
          index
        ) in movies"
        :key="index"
        class="movie"
      >
        <div class="movie-img">
          <img
            :src="
              poster_path
                ? `https://image.tmdb.org/t/p/w500${poster_path}`
                : noImage
            "
            alt=""
          />
          <p class="review">{{ vote_average }}</p>
          <p class="overview">
            {{ overview.substring(0, 200)
            }}<span v-if="overview.length > 200">...</span>
          </p>
        </div>
        <div class="info">
          <p class="title">
            {{ title.slice(0, 25) }}<span v-if="title.length > 25">...</span>
          </p>
          <p class="release">Released: {{ dateFormatted(release_date) }}</p>
          <NuxtLink
            class="button button-light"
            :to="{ name: 'movies-movieid', params: { movieid: id } }"
          >
            Get More Info
          </NuxtLink>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Movies',
  props: {
    movies: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      noImage: 'https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/No-Image-Placeholder.svg/1200px-No-Image-Placeholder.svg.png',
    }
  },
  head() {
    return {
      title: 'Nuxty - Movie info searcher',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, stremaing',
        },
      ],
    }
  },
  methods: {
    dateFormatted(date) {
      return new Date(date).toLocaleString('en-us', {
        month: 'long',
        day: 'numeric',
        year: 'numeric',
      })
    },
  },
}
</script>
<style lang="scss">
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }
  .search {
    display: flex;
    padding: 32px 16px;
    input {
      max-width: 350px;
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;
      border-radius: 20px;
      &:focus {
        outline: none;
      }
    }
    .button {
      margin-left: 16px;
    }
  }
  .movies {
    padding: 32px 16px;
    .movies-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(5, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
        .movie-img {
          position: relative;
          overflow: hidden;
          &:hover {
            .overview {
              transform: translateY(0);
            }
          }
          img {
            display: block;
            width: 100%;
            height: 100%;
            border-radius: 20px;
          }
          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 40px;
            height: 40px;
            background-color: #c92502;
            color: #fff;
            border-radius: 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
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
        .info {
          margin-top: auto;
          .title {
            margin-top: 12px;
            color: #fff;
            font-size: 20px;
          }
          .release {
            margin-top: 8px;
            color: #c9c9c9;
          }
          .button {
            margin-top: 8px;
          }
        }
      }
    }
  }
}
</style>