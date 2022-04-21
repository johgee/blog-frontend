<template>
  <div class="home">
    <h1>New Post</h1>
    <form v-on:submit.prevent="createPost()">
      <ul>
        <li v-for="error in errors" v-bind:key="error" style="color: green">
          {{ error }}
        </li>
      </ul>
      <p>
        Title:
        <input type="text" v-model="newPost.title" />
      </p>
      <p>
        <small v-if="newPost?.title?.length > 0 && newPost?.title?.length <= 20">
          Remaining characters: {{ 20 - newPost?.title?.length }}
        </small>
        <small v-if="newPost?.title?.length > 20" class="text-danger">Post title can't be over 20 characters.</small>
      </p>
      <p>
        Body:
        <input type="text" v-model="newPost.body" />
      </p>
      <p>
        Image:
        <input type="text" v-model="newPost.image" />
      </p>
      <input type="submit" value="Create Post" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newPost: {},
      errors: [],
    };
  },
  methods: {
    createPost() {
      axios
        .post("/posts", this.newPost)
        .then((response) => {
          console.log(response);
          this.$router.push("/posts");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(this.errors);
        });
    },
  },
};
</script>
