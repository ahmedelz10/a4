<template>
  <div>
    <h1>The Movies Database (TMDB)</h1>
    <hr />
    <br />
    <div id="m_dropdown">
      <label for="m_movies">Select a movie from the dropdown menu:</label>
      <br />
      <select id="m_movies" v-model="selectedMovie">
        <option value=""></option>
        <option v-for="movie in movies" :key="movie.id" :value="movie.id">
          {{ movie.title }}
        </option>
      </select>
      <button id="m_get_button" @click="getMovie">GET</button>
    </div>
    <br />
    <div id="m_information" v-if="movieData">
      <h2>Movie Information</h2>
      <h3>Movie Title:</h3>
      <p id="m_title">{{ movieData.title }}</p>
      <h3>Movie Release Date:</h3>
      <p id="m_release">{{ movieData.release_date }}</p>
      <h3>Budget:</h3>
      <p id="m_budget">{{ formatCurrency(movieData.budget) }}</p>
      <h3>Revenue:</h3>
      <p id="m_revenue">{{ formatCurrency(movieData.revenue) }}</p>
      <h3>Film Overview:</h3>
      <p id="m_overview">{{ movieData.overview }}</p>
      <h3>Popularity Rating:</h3>
      <p id="m_popularity">{{ movieData.popularity }}</p>
      <h3>User Score:</h3>
      <p id="m_score">{{ calculateScore(movieData.vote_average) }}</p>
      <h3>Tagline:</h3>
      <p id="m_tagline">{{ movieData.tagline }}</p>
    </div>
    <div id="m_media" v-if="movieData">
      <h2>Movie Media</h2>
      <iframe
        id="m_video"
        width="560"
        height="315"
        frameborder="0"
        allowfullscreen
        :src="getTrailerUrl()"
      ></iframe>
      <img
        id="m_image"
        alt=""
        :src="`https://image.tmdb.org/t/p/w500${movieData.poster_path}`"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      selectedMovie: "",
      movies: [
        { id: "299536", title: "Avengers: Infinity War" },
        { id: "299534", title: "Avengers: Endgame" },
        { id: "384018", title: "Fast & Furious Presents: Hobbs & Shaw" },
        { id: "634649", title: "Spider-Man: No Way Home" },
        { id: "361743", title: "Top Gun: Maverick" },
        { id: "101299", title: "The Hunger Games: Catching Fire" },
        { id: "580489", title: "Venom: Let There Be Carnage" },
        { id: "7214", title: "Coach Carter" },
        { id: "772", title: "Home Alone 2: Lost in New York" },
        { id: "566525", title: "Shang-Chi and the Legend of the Ten Rings" },
      ],
      movieData: null,
    };
  },
  methods: {
    async getMovie() {
      if (this.selectedMovie) {
        const response = await axios.get(
          `https://api.themoviedb.org/3/movie/${this.selectedMovie}`,
          {
            params: {
              api_key: "413ca7087241a73a9022161f8a6dce66",
              append_to_response: "videos",
            },
          }
        );
        this.movieData = response.data;
      }
    },
    formatCurrency(value) {
      return new Intl.NumberFormat().format(value);
    },
    calculateScore(value) {
      return parseInt(value) * 10 + "%";
    },
    getTrailerUrl() {
      if (this.movieData && this.movieData.videos) {
        const trailers = this.movieData.videos.results.filter(
          (trailer) => trailer.type === "Trailer"
        );
        if (trailers.length > 0) {
          return `https://www.youtube.com/embed/${trailers[0].key}`;
        }
      }
      return "";
    },
  },
};
</script>

<style scoped>
p {
  color: rgb(216, 19, 19);
  font-weight: bold;
  font-family: "Poor Story", cursive;
  font-size: 20px;
}

h1 {
  font-size: 36px;
  font-family: "Arvo", serif;
}

h3 {
  font-size: 20px;
  text-decoration: underline;
  font-family: "Pacifico", cursive;
}

#m_title {
  font-family: "Lobster", cursive;
  font-size: 40px;
  text-align: center;
  margin-bottom: 10px;
  color: rgb(0, 0, 0);
}

#m_movies {
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  font-size: 20px;
}

#m_image {
  height: auto;
  width: 300px;
}

iframe {
  height: 400px;
  aspect-ratio: 16 / 9;
  width: 100%;
}

#m_movies {
  height: 30px;
  width: 500px;
  text-align: center;
}

#m_get_button {
  height: 30px;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background-color: lightblue;
  background-image: url("gradientbackground.svg");
  height: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

#m_information {
  width: 45%;
  margin-right: 1%;
  float: left;
  padding: 2%;
  border: 2px solid rgb(5, 26, 145);
}

#m_media {
  width: 45%;
  display: inline-block;
  padding: 2%;
  border: 2px solid rgb(0, 0, 0);
}

option {
  font-size: 16px;
  font-family: Georgia, "Times New Roman", Times, serif;
}
</style>
