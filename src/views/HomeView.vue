<template>
  <div class="home">
    <h1 v-bind:class="className">{{ message }} count: {{ posts.length }}</h1>
    <h2>New Post</h2>
    <p>
      Title:
      <input type="text" v-model="newPost.title" />
    </p>
    <p>
      Body:
      <input type="text" v-model="newPost.body" />
    </p>
    <p>
      Image:
      <input type="text" v-model="newPost.image" />
    </p>
    <button v-on:click="createPost()">Create Post</button>
    <div v-for="post in posts" v-bind:key="post.id">
      <h3>{{ post.title }}</h3>
      <p>body: {{ post.body }}</p>
      <p>image: {{ post.image }}</p>
      <img v-bind:src="post.image_url" v-bind:alt="post.title" style="max-width: 250px" />
      <div>
        <button v-on:click="showPost(post)">More info</button>
      </div>
    </div>

    <dialog id="post-details">
      <form method="dialog">
        <h1>Post info</h1>
        <p>Title: {{ currentPost.title }}</p>
        <p>Body: {{ currentPost.body }}</p>
        <p>Image: {{ currentPost.Image }}</p>
        <h1>Edit Post</h1>
        <p>
          title:
          <input v-model="currentPost.title" type="text" />
        </p>
        <p>
          body:
          <input v-model="currentPost.body" type="text" />
        </p>
        <p>
          image:
          <input v-model="currentPost.image" type="text" />
        </p>
        <button v-on:click="updatePost()">Update</button>
        <button v-on:click="deletePost(currentPost)">Delete Post</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "this is my blog!",
      posts: [],
      newPostParams: {},
      editPostParams: {},
      currentPost: {},
    };
  },
  created: function () {
    this.indexPosts();
  },
  methods: {
    indexPosts: function () {
      axios.get("/posts").then((response) => {
        console.log(response.data);
        this.posts = response.data;
      });
    },
    createPost() {
      axios.post("/posts", this.newPost).then((response) => {
        this.posts.push(response.data);
      });
    },
    showPost(post) {
      axios.get(`/posts/${post.id}`).then((response) => {
        this.currentPost = response.data;
        this.editPost = response.data;
        document.querySelector("#post-details").showModal();
      });
    },
    updatePost() {
      axios.patch(`/posts/${this.currentPost.id}`, this.currentPost).then((response) => {
        console.log("Success!", response);
        var index = this.posts.indexOf(this.currentpost);
        this.posts.splice(index, 1);
        this.posts.push(response.data);
      });
    },
    destroyPost(post) {
      axios.delete(`/posts/${post.id}`).then((response) => {
        console.log(response);
        var index = this.posts.indexOf(post);
        this.posts.splice(index, 1);
      });
    },
  },
};
</script>

<style></style>
