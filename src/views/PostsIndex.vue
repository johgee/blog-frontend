<template>
  <div class="home">
    <h1 v-bind:class="className">{{ message }} count: {{ posts.length }}</h1>
    <div v-for="post in posts" v-bind:key="post.id">
      <h3>{{ post.title }}</h3>
      <p>Body: {{ post.body }}</p>
      <img v-bind:src="post.image" v-bind:alt="post.title" style="max-width: 250px" />
      <div>
        <router-link class="button" v-bind:to="`/posts/${post.id}`">More Info</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "my posts",
      posts: [],
    };
  },
  created: function () {
    this.indexPosts();
  },
  methods: {
    indexPosts() {
      axios.get("/posts").then((response) => {
        console.log(response.data);
        this.posts = response.data;
      });
    },
  },
};
</script>

<style>
.green {
  color: green;
}
.button {
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
</style>
