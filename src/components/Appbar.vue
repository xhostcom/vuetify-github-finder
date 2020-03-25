<template>
   <v-app-bar
      app
      color="primary"
      dark
    >
    <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="../assets/octo.webp"
          transition="scale-transition"
          width="40"
        />
        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="140"
          src="../assets/gitfinder.webp"
          width="140"
        />
       </div>
       <v-spacer></v-spacer>
       <v-text-field
          class="mt-6"
          v-model="localValue"
          autofocus
          @keypress="fetchData"
          label="Enter Github User Name"
          append-icon="search"
          ></v-text-field>
           <v-spacer></v-spacer>
        <v-btn
        to="/"
        target="_blank"
        text
      >
      <v-icon>mdi-refresh</v-icon>
      </v-btn>
    </v-app-bar>
</template>
<script>
  export default {
    name: 'Appbar',
    data: () => ({ }),
  }
</script>
