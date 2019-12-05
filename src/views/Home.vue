<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="12">
        <!-- === Card: Header, Input and Clear btn === -->
        <v-card color="orange lighten-1">
          <!-- TITLE -->
          <v-card-title class="display-1 orange white--text font-weight-bold">OMDB API</v-card-title>
          <!-- Little description -->
          <v-card-text
            class="white--text headline"
          >Exercice évaluation recrutement R&D ip-label (vue.js)</v-card-text>
          <!-- Input text -->
          <v-card-text>
            <!-- === Text field (term) === -->
            <v-text-field
              label="Nom du Film"
              color="white"
              v-model="query.term"
              @keyup="termChanged"
            ></v-text-field>

            <v-divider></v-divider>

            <!-- === Clear btn === -->
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn :disabled="!query.term" @click="clear">
                Effacer
                <v-icon right>mdi-close-circle</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card-text>

          <!-- === Datatable === -->
          <v-data-table
            :headers="datatable.headers"
            :items="movies"
            :items-per-page="10"
            :loading="datatable.loading"
            :hide-default-footer="true"
            class="elevation-1"
            @click:row="clicked"
          ></v-data-table>
        </v-card>

        <!-- === Pagination === -->
        <v-pagination
          color="orange"
          v-model="query.pagination.current"
          :length="query.pagination.total"
          @input="onPageChange"
        ></v-pagination>
      </v-col>
    </v-row>

    <!-- MODAL DIALOG (show one movie) -->
    <Modal :selectedMovie="selectedMovie" :show="showDialog" @modal-close="showDialog = false"></Modal>
  </v-container>
</template>

<script>
import Modal from "../components/Modal";

export default {
  name: "home",
  components: { Modal },
  data() {
    return {
      url: "https://www.omdbapi.com/?apikey=8f0a5987&s=",
      url_movie: "https://www.omdbapi.com/?apikey=8f0a5987&plot=full&t=",
      query: {
        term: "",
        t: "",
        pagination: {
          current: 1,
          total: 0
        }
      },
      movies: [],
      selectedMovie: {
        Title: "",
        Poster: "",
        Year: "",
        Type: "",
        Awards: "",
        Plot: "",
        Genre: ""
      },
      datatable: {
        headers: [
          { text: "Titre", value: "Title" },
          { text: "Année", value: "Year" }
        ],
        totalResults: 0,
        loading: false
      },
      showDialog: false
    };
  },
  methods: {
    // When click on clear btn
    clear: function() {
      this.query.term = null;
      this.movies = [];
      this.query.pagination.total = 0;
      this.query.pagination.current = 1;
    },
    // When the term change
    termChanged: function() {
      // no search when term < 3
      if (this.query.term.trim().length < 3) {
        this.reset();
      }
      if (this.query.term.trim().length >= 3) {
        this.searchOmdb();
      }
    },
    // click on row (show dialog)
    searchOmdb: async function() {
      this.datatable.loading = true;

      const response = await fetch(
        `${this.url}${this.query.term.trim()}&page=${
          this.query.pagination.current
        }`
      );
      const moviesResponse = await response.json();
      // Handdle Error
      if (moviesResponse.Error) return null;

      // movies data
      this.movies = moviesResponse.Search;
      // Total results (pagination)
      this.datatable.totalResults = parseInt(moviesResponse.totalResults);
      // nbr of pages
      this.query.pagination.total = Math.ceil(moviesResponse.totalResults / 10);

      this.datatable.loading = false;
    },
    // When paginate
    onPageChange: function() {
      this.searchOmdb();
    },
    // clicked on a table row (show dialog)
    clicked: async function(value) {
      const response = await fetch(`${this.url_movie}${value.Title}`);
      const selectedMovieRequest = await response.json();

      this.selectedMovie = selectedMovieRequest;

      this.showDialog = true;
    },
    // reset the application (term, pagination and movies)
    reset: function() {
      this.datatable.loading = false;
      this.query.pagination.total = 0;
      this.query.pagination.current = 1;
      this.movies = [];
    }
  }
};
</script>
