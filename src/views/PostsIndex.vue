<template>
  <div class="home row">
    <h1 class="m-3">{{ message }}</h1>
    <div
      class="col"
      v-bind:class="{ selected: post === currentPost }"
      c-for="post in posts"
      v-bind:key="post.id"
      v-on:click="currentPost = post"
    >
      <div class="card" style="width: 18rem">
        <img v-bind:src="post.image" class="card-img-top" v-bind:alt="post.title" />
        <div class="card-body">
          <h5 class="card-title">{{ post.title }}</h5>
          <p class="card-text">Body: {{ post.body }}</p>
          <a v-bind:href="`/posts/${post.id}`" class="btn btn-primary">More Info</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "all posts",
      posts: [],
      currenPost: {},
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
green {
  color: green;
}
.selected .card-body {
  color: white;
  background-color: burlywood;
}
img {
  heoght: 250px;
  object-fot: cover;
}
button {
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
