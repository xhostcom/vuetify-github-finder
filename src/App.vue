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
        <v-icon>mdi-refresh mdi-36px</v-icon>
      </v-btn>
    </v-toolbar>
  </div>
     <v-content>
      <v-layout class="profile-details" mt-12 mb-12 justify-center align-center>
       <v-card flat light justify-center align-center>
        <h2 class="user-title text-center" justify-center align-center>{{ details.name }}</h2>
        <img class="user-img" :src="details.avatar_url" />
          <v-card-text>{{ details.bio }}</v-card-text>
          <v-card-body>
           <v-btn  v-model="hide" to="/" color="info">View Profile</v-btn>
          </v-card-body>
           <br />
           <br />
           </v-card>
           </v-layout>
    <v-layout class="profile-details" mt-12 mb-12 justify-center align-center>
    <v-badge
     color="secondary"
     left>
      <template v-slot:badge>
        <span>{{ details.public_repos }}</span>
      </template>
      <span><strong> Public Repos </strong></span>
    </v-badge>
     &nbsp;&nbsp;
     <v-badge
     color="primary"
     left>
      <template v-slot:badge>
        <span>{{ details.public_gists }}</span>
      </template>
      <span><strong> Public Gists  </strong></span>
    </v-badge>
     &nbsp;&nbsp;
     <v-badge
     color="success"
     left>
      <template v-slot:badge>
        <span>{{ details.followers }}</span>
      </template>
      <span><strong> Followers  </strong></span>
    </v-badge>
     &nbsp;&nbsp;
     <v-badge
     color="warning"
     left>
      <template v-slot:badge>
        <span>{{ details.following }}</span>
      </template>
      <span><strong> Following </strong></span>
    </v-badge>
    </v-layout>
     <v-layout class="profile-details" mt-12 mb-12 justify-center align-center>
    <v-row>
      <v-card>
      <v-list>
        <v-list-item-group color="primary">
          <v-list-item>
           Company
          </v-list-item>
            <v-list-item-content>
              <v-list-item-title>{{ details.company }}</v-list-item-title>
           </v-list-item-content>
            <v-list-item>
           Company
          </v-list-item>
            <v-list-item-content>
              <v-list-item-title>{{ details.company }}</v-list-item-title>
           </v-list-item-content>
            <v-list-item>
           Company
          </v-list-item>
            <v-list-item-content>
              <v-list-item-title>{{ details.company }}</v-list-item-title>
           </v-list-item-content>
            <v-list-item>
           Company
          </v-list-item>
            <v-list-item-content>
              <v-list-item-title>{{ details.company }}</v-list-item-title>
           </v-list-item-content>
        </v-list-item-group>
      </v-list>
    </v-card>
  </v-row>
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
.profile-details {
  text-align:center;
  justify-content: center;
  margin: 0 auto;
  width: 100%;
  max-width: 768px;
}
.user-img {
  width:250px;
  border-radius: 50%;
  margin-top: 10px;
}
.repodata,
.profile-details {
display: none;
}
.repodata,
/*.profile-details {
  width: 100%;
  text-align: center;
  margin: 0 auto;
  max-width: 768px;
}*/
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
