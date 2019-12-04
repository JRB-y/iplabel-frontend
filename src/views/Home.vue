<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="12">
        <!-- === Card: Header, Input and Clear btn === -->
        <v-card color="yellow lighten-2" flat>
          <!-- TITLE -->
          <v-card-title class="headline yellow">
            OMDB API
          </v-card-title>
          <!-- Little description -->
          <v-card-text>
            Exercice évaluation recrutement R&D ip-label (vue.js)
          </v-card-text>

          <!-- Input text -->
          <v-card-text>
            <v-text-field
              label="Nom du Film"
              color="black"
              v-model="term"
              @keyup="termChanged"
            ></v-text-field>

            <v-divider></v-divider>

            <!-- clear term -->
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn :disabled="!term" @click="clear">
                Effacer
                <v-icon right>mdi-close-circle</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card-text>
        </v-card>

        <!-- === Datatable === -->
        <v-data-table
          :headers="headers"
          :items="movies"
          :items-per-page="10"
          :server-items-length="totalResults"
          :loading="loading"
          :hide-default-footer="true"
          class="elevation-1"
          @click:row="clicked"
        ></v-data-table>

        <!-- === Pagination === -->
        <v-pagination
          color="yellow"
          v-model="pagination.current"
          :length="pagination.total"
          @input="onPageChange"
        ></v-pagination>
      </v-col>
    </v-row>

    <!-- MODAL DIALOG -->
    <Modal
      :selectedMovie="selectedMovie"
      :show="show"
      @modal-close="show = false"
    ></Modal>
  </v-container>
</template>

<script>
import Modal from "../components/Modal";

export default {
  name: "home",
  components: { Modal },
  data() {
    return {
      term: "",
      url: "http://www.omdbapi.com/?apikey=8f0a5987&s=",
      headers: [
        { text: "Titre", value: "Title" },
        { text: "Année", value: "Year" }
      ],
      movies: [],
      totalResults: 0,
      loading: false,
      pagination: {
        current: 1,
        total: 0
      },
      show: false,
      selectedMovie: {
        Title: "",
        Poster: "",
        Year: "",
        Type: "",
        Awards: "",
        Plot: "",
        Genre: ""
      }
    };
  },
  methods: {
    clear: function() {
      this.term = null;
      this.movies = [];
      this.pagination.total = 0;
      this.pagination.current = 1;
    },
    termChanged: function() {
      if (this.term.trim().length < 3) {
        this.loading = false;
        this.pagination.total = 0;
        this.pagination.current = 1;
        this.movies = [];
      }
      if (this.term.trim().length >= 3) {
        this.searchOmdb();
      }
    },
    searchOmdb: async function() {
      // if (this.pagination.current == this.pagination.total) return null;

      this.loading = true;
      const response = await fetch(
        `${this.url}${this.term.trim()}&page=${this.pagination.current}`
      );
      const moviesResponse = await response.json();

      if (moviesResponse.Error) {
        console.log(moviesResponse.Error);
        return null;
      }

      this.movies = moviesResponse.Search;

      this.totalResults = parseInt(moviesResponse.totalResults);
      this.pagination.total = Math.ceil(moviesResponse.totalResults / 10);

      this.loading = false;
    },
    onPageChange: function() {
      this.searchOmdb();
    },
    // clicked on a table row (display a single movie)
    clicked: async function(value) {
      const response = await fetch(
        `http://www.omdbapi.com/?apikey=8f0a5987&plot=full&t=${value.Title}`
      );
      const selectedMovieRequest = await response.json();
      console.log(this.selectedMovie);

      this.selectedMovie = selectedMovieRequest;
      this.show = true;
    }
  }
};
</script>
