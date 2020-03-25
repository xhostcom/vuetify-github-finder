<template>
  <v-app>
     <div>
    <v-toolbar
      dark
      src="./assets/bg.webp"
      >
      <v-btn
       to="/"
       target="_blank"
       text
       icon>
        <v-icon>mdi-github-circle mdi-48px </v-icon>
          </v-btn>
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
       icon>
        <v-icon>mdi-refresh</v-icon>
      </v-btn>
    </v-toolbar>
  </div>
     <v-content>
      <v-layout row wrap mt-12 mb-12 justify-center align-center class="profile-search">
      <v-container xs6 mt-12 mb-12 class="profile-details text-center">
        <v-card>
          <h2 class="user-title">{{ details.name }}</h2>
        </v-card>
          <img class="user-img" :src="details.avatar_url" width="200px;" />
         <br />
         <br />
         <v-btn class="btn-dark btn-lg btn-block"><a href="#" style="color: #fff; text-decoration:none;">View Profile</a></v-btn>
          <br />
           <v-container xs6 mt-12 mb-12>
            <v-card class="text-center">
              <v-card-body>{{ details.bio }}</v-card-body>
           </v-card>
          </v-container>
           <br />
               <v-container xs6 mt-12 mb-12>
              <span class="badge badge-secondary"><strong>Public Repos:</strong>{{ details.public_repos }}</span>
              <span class="badge badge-primary"><strong>Public Gists:</strong>{{ details.public_gists }}</span>
              <span class="badge badge-success"><strong>Followers:</strong>{{ details.followers }}</span>
              <span class="badge badge-info"><strong>Following:</strong>{{ details.following }}</span>
              <br><br>
              <ul class="list-group">
             <li class="list-group-item d-flex justify-content-between align-items-center">Company: {{ details.company }}</li>
             <li class="list-group-item d-flex justify-content-between align-items-center">Website/blog: <a href="#" target="_blank">{{ details.blog }}</a></li>
             <li class="list-group-item d-flex justify-content-between align-items-center">Location: {{ details.location }}</li>
             <li class="list-group-item d-flex justify-content-between align-items-center">Member Since: {{ details.created_at }}</li>
           </ul>
          </v-container>
        </v-container>
     </v-layout>
    </v-content>
  </v-app>
</template>

<script>
export default {
 name: 'App',
   props: {
    value: String
  },
    components: {
  },
  data: () => ({
  localValue: "",
  details: {},
  avatar_url: "",
  url_base: 'https://api.github.com/users/',
  index: 0
  }),
  created () {
      this.localValue = this.value;
      this.$watch("localValue", value => {
      this.$emit("input", value);
  });
 },
 methods: {
        fetchData(e) {
        if (e.key == "Enter") {
        fetch(`${this.url_base}${this.localValue}`)
        .then(res => {
        return res.json();
        }).then(this.setResults)
      }
       this.getRepos();
    },
     setResults(results) {
     this.details = results;
     document.querySelector(".profile-details").style.display = "block";
     document.querySelector(".repodata").style.display = "block";
 }
}
}
</script>
<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: sans-serif;
}
.user-img {
  border-radius: 50%;
  margin-top: 10px;
}
.repodata,
.profile-details {
display: none;
}
.profile-search {
  margin-top:40px;
}
.repodata,
.profile-details {
  width: 100%;
  text-align: center;
  margin: 0 auto;
  margin-bottom: 30px;
  max-width: 768px;
}
.repo-name {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.repo-name span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
}
.repos {
  padding: 0px 30px;
}
.repos h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.repos .repo {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.repos .repo.playing {
  color: #FFF;
 background-color: #2d3436;
background-image: linear-gradient(315deg, #2d3436 0%, #000000 74%);
}
</style>
