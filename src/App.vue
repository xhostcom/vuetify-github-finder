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
        <v-icon>mdi-refresh mdi-24px</v-icon>
      </v-btn>
    </v-toolbar>
  </div>
     <v-content>
     <v-container class="profile-details">
     <v-layout row wrap justify-center align-center mt-6>
      <v-flex xs12>
       <v-card flat light justify-center align-center>
        <h2 class="user-title text-center" justify-center align-center>{{ details.name }}</h2>
        <img class="user-img" :src="details.avatar_url" />
          <v-card-text>{{ details.bio }}</v-card-text>
          <v-card-body>
           <v-btn mt-6 to="/" color="secondary">View Profile</v-btn>
          </v-card-body>
           <br />
           <br />
      </v-card>
      </v-flex>
     </v-layout>
     </v-container>
    <v-container class="top-details">
    <v-layout row wrap justify-center align-center>
    <v-flex xs12>
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
      </v-flex>
   </v-layout>
 </v-container>
  <v-container class="details">
     <v-layout class="details" row wrap justify-center align-center>
      <v-flex xs12>

    <v-alert
      class="alert"
      border="left"
      color="red lighten-2"
      dark
      dense
    >
     <span pl-4><strong> Company: </strong></span>   <span>{{ details.company }}</span>
    </v-alert>
    <v-alert
      class="alert"
      border="right"
      color="blue-grey"
      dark
      dense
    >
      <span><strong> Website: </strong></span>   <span>{{ details.blog }}</span>
    </v-alert>
    <v-alert
      class="alert"
      border="left"
      color="pink darken-1"
      dark
      dense
    >
     <span><strong> Location: </strong></span>   <span>{{ details.location }}</span>
    </v-alert>
    <v-alert
      class="alert"
      border="right"
      color="indigo"
      dark
      dense
    >
   <span pr-4><strong> Member Since: </strong></span>   <span>{{ details.created_at }}</span>
    </v-alert>

      </v-flex>
 </v-layout>
</v-container>
   <v-container class="repodata">
     <v-layout row wrap justify-center align-center >
     <v-flex xs12>
      <v-btn mr-4 to="/" color="success" class="prev" @click="prev">Prev</v-btn>
     <v-btn ml-4 to="/" color="success" class="next" @click="next">Next</v-btn>
     <br />
     <br />
     <h2>Latest Repos</h2>
   <br />
  <!--<h4 v-for="repo in repoData" v-bind:key="repo.index" class="repo-name">{{ repoData.title }} - <span>{{ repoData.description }}</span></h4>-->

  </v-flex>
 </v-layout>
 </v-container>
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
  index: 0,
  id: "",
  isViewing: false,
  current: {},
  repos: [],
  }),
  created () {
      this.localValue = this.value;
      this.$watch("localValue", value => {
      this.$emit("input", value);
  });
   this.current = this.repos[this.index];
   this.repo = this.repos[this.id];
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
     document.querySelector(".top-details").style.display = "block";
     document.querySelector(".details").style.display = "block";
     document.querySelector(".profile-details").style.display = "block";
     document.querySelector(".repodata").style.display = "block";
    },
     getRepos() {
         fetch(`${this.url_base}${this.localValue}` + '/repos')
          .then(res => res.json())
          .then(data => { this.repos = data })
          .then(this.setRepos)

      },
      setRepos() {
         const repoData = [
            {
            id:this.repos.id,
            },
            {
            title:this.repos.name,
            },
            {
            description:this.repos.description,
            }
          ]
         console.log("It Works, heres the data!");
         console.log(repoData);
      },
  prev() {},
  next() {}
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
.prev,
.next {
  margin:8px;
}
.details {
margin: 0 auto;
}
.alert {
width: 90%;
margin: 0 auto;
}
.top-details,
.repodata,
.profile-details {
  text-align:center;
  justify-content: center;
  margin: 0 auto;
  width: 600px;
  max-width: 600px;
}
.user-img {
  width:250px;
  border-radius: 50%;
  margin-top: 10px;
}
.top-details,
.details,
.repodata,
.profile-details {
display: none;
}
.repo-name {
  color: #53565A;
  font-size: 18px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.repo-name span {
  font-weight: 400;
  font-style: italic;
}
.repos {
  padding: 0px 30px;
}
.repos .repo {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.repos .repo.viewing {
color: #FFF;
background-color: #2d3436;
background-image: linear-gradient(315deg, #2d3436 0%, #000000 74%);
}
</style>
