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
           <v-btn to="/" color="secondary">View Profile</v-btn>
          </v-card-body>
           <br />
           <br />
      </v-card>
     </v-layout>
    <v-layout class="profile-details" mt-12 mb-12 justify-center align-center>
    <v-badge
     dense
     color="secondary"
     left>
      <template v-slot:badge>
      <span>{{ details.public_repos }}</span>
      </template>
      <span pr-4><strong> Public Repos </strong></span>
     </v-badge>
     <v-badge
     dense
     color="primary"
     left>
      <template v-slot:badge>
        <span>{{ details.public_gists }}</span>
      </template>
      <span pr-4><strong> Public Gists  </strong></span>
    </v-badge>
     <v-badge
     dense
     color="success"
     left>
      <template v-slot:badge>
      <span>{{ details.followers }}</span>
      </template>
      <span pr-4><strong> Followers  </strong></span>
    </v-badge>
     <v-badge
     dense
     color="warning"
     left>
      <template v-slot:badge>
        <span>{{ details.following }}</span>
      </template>
      <span pr-4><strong> Following </strong></span>
    </v-badge>
    </v-layout>
     <v-layout class="profile-details" mt-12 mb-12 justify-center align-center>
    <v-alert
      border="left"
      color="red lighten-2"
      dark
    >
     <span><strong> Company: </strong></span>   <span>{{ details.company }}</span>
    </v-alert>
    <v-alert
      border="right"
      color="blue-grey"
      dark
    >
      <span><strong> Website: </strong></span>   <span>{{ details.blog }}</span>
    </v-alert>
    <v-alert
      border="left"
      color="pink darken-1"
      dark
    >
     <span><strong> Location: </strong></span>   <span>{{ details.location }}</span>
    </v-alert>
    <v-alert
      border="right"
      color="indigo"
      dark
    >
      <span><strong> Member Since: </strong></span>   <span>{{ details.created_at }}</span>
    </v-alert>
 </v-layout>
  <v-layout class="repodata" mt-12 mb-12 justify-center align-center>
  <div class="controls">
          <v-btn to="/" class="btn-success btn-lg prev" @click="prev">Prev</v-btn>
          <v-btn to="/" class="btn-success btn-lg next" @click="next">Next</v-btn>
        </div>
           <v-card class="text-center">
           <v-card-title><h2 class="text-center section-title">Latest Repos</h2></v-card-title>
           <h4 v-for="repo in repos" v-bind:key="repo.id" class="repo-name">{{ repo.name }} - <span>{{ repo.description }}</span></h4>
         <div>
    <v-alert
      border="left"
      color="red lighten-2"
      dark
    >
     <span><strong> Created: </strong></span>   <span>{{ details.company }}</span>
    </v-alert>
    <v-alert
      border="right"
      color="blue-grey"
      dark
    >
      <span><strong> Updated: </strong></span>   <span>{{ details.blog }}</span>
    </v-alert>
    <v-alert
      border="left"
      color="pink darken-1"
      dark
    >
     <span><strong> Forks: </strong></span>   <span>{{ details.location }}</span>
    </v-alert>
    <v-alert
      border="right"
      color="indigo"
      dark
    >
      <span><strong> Watchers: </strong></span>   <span>{{ details.created_at }}</span>
    </v-alert>
     <v-alert
      border="right"
      color="blue"
      dark
    >
      <span><strong> Stars: </strong></span>   <span>{{ details.created_at }}</span>
    </v-alert>
  </div>
           </v-card>
           <br />
            <v-btn to="/" color="success">View Repository</v-btn>
 </v-layout>
 </v-content>
  <Footer />
  </v-app>
</template>
<script>
import Footer from '@/components/Footer';
export default {
 name: 'App',
   props: {
    value: String
  },
    components: {
      Footer
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
    },
     getRepos() {
         fetch(`${this.url_base}${this.localValue}` + '/repos')
          .then(res => res.json())
          .then(data => { this.repos = data });
           console.log(this.repos);
      },
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
.repodata,
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
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
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
