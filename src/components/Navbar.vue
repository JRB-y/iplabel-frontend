<template>
  <nav>
    <v-app-bar app dark flat>
      <v-toolbar-title>
        <b>IP LABEL</b>
        <span class="orange--text ml-1">omdb API</span>
      </v-toolbar-title>

      <v-spacer></v-spacer>
      <v-btn href="https://github.com/JRB-y/iplabel-frontend" target="_blank" text>
        <span class="mr-2 grey--text text--lighten-4">On github</span>
        <v-icon class="grey--text text--lighten-4">mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      showMenu: false
    };
  }
};
</script>
