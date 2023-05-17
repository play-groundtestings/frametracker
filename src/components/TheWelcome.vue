<template>
  <div>
    <ul v-for="post in posts" v-bind:key="post.id">
      <li><b>title: </b>{{ post.title }}</li>
      <p><b>body: </b>{{ post.body }}</p>
      <hr>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [],
    };
  },

  methods: {
    async getData() {
      try {
        let response = await fetch("http://jsonplaceholder.typicode.com/posts");
        this.posts = await response.json();;
        console.log(typeof(this.posts))
        //console.log(this.posts[0]['userId'])
      } catch (error) {
        console.log(error);
      }
    },
  },

  created() {
    this.getData();
  },
};
</script>