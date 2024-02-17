<template>
  <div>
    <div v-for="post in posts" :key="post.id">
      <img :src="post.media_url" alt="Instagram Post" />
      <p>{{ post.caption }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      posts: []
    };
  },
  created() {
    this.fetchUserPosts();
  },
  methods: {
    async fetchUserPosts() {
      try {
        const response = await axios.get('https://graph.instagram.com/me/media', {
          params: {
            fields: 'id,media_url,caption',
            access_token: 'YOUR_ACCESS_TOKEN'
          }
        });
        this.posts = response.data.data;
      } catch (error) {
        console.error('Error fetching user posts:', error);
      }
    }
  }
};
</script>
