<template>
  <v-dialog v-model="show" max-width="580" @click:outside="closeDialog">
    <v-card class="mx-auto" max-width="580">
      <v-img height="250" :src="selectedMovie.Poster"></v-img>

      <v-card-title>{{ selectedMovie.Title }}</v-card-title>

      <v-card-text>
        <div class="my-4 subtitle-1 black--text">{{ selectedMovie.Awards }}</div>

        <div>{{ selectedMovie.Plot }}.</div>
      </v-card-text>

      <v-divider class="mx-4"></v-divider>

      <v-card-title>Genre</v-card-title>

      <v-card-text>
        <v-chip-group active-class="orange white--text" column>
          <v-chip>{{ selectedMovie.Genre }}</v-chip>
        </v-chip-group>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "Modal",
  props: ["selectedMovie", "show"],
  methods: {
    closeDialog: function() {
      this.$emit("modal-close", this.show);
    }
  }
};
</script>
