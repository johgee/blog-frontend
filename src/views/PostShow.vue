<template>
  <div class="home">
    <h1>Post Info</h1>
    <p>Title: {{ post.title }}</p>
    <img v-bind:src="post.image_url" v-bind:alt="post.title" style="max-width: 250px" />
    <p>Body: {{ post.body }}</p>
    <p>Image: {{ post.image }}</p>
    <router-link to="/posts" style="marin-right: 10px">Back to all posts</router-link>
    <router-link v-bind:to="`/posts/${post.id}/edit`" style="margin-right: 10px">Edit Post</router-link>
    <button v-on:click="destroyPost()">Delete</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      this.post = response.data;
    });
  },
  methods: {
    destroyPost() {
      axios.delete(`/posts/${this.post.id}`).then((response) => {
        console.log(response);
        this.$router.push("/posts");
      });
    },
  },
};
</script>
