<template>
  <div>
    <div class="post-grid">
      <div class="post-card" v-for="post in posts" :key="post.id">
        <div class="post-image">
          <img :src="post.media_url" alt="Instagram Post" />
        </div>
        <div class="post-title">
          <p>{{ post.caption }}</p>
        </div>
      </div>
    </div>
    <div class="loading" v-if="loading">Loading...</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      loading: false,
      posts: [],
    };
  },
  created() {
    this.fetchUserPosts();
  },
  methods: {
    async fetchUserPosts() {
      this.loading = true;
      try {
        const response = await axios.get(
          "https://graph.instagram.com/me/media",
          {
            params: {
              fields: "id,media_url,caption",
              access_token:
                "IGQWRQUjBJZAkpveW1nX2g5clJoV1NDUnpDVHh4aVQ0cGhBV0RaaTVJY3UxQVNrVnJVWTNuVU5XZA1RsM1JJTjFWY1RRMmgxVVVuQ1UtZA2lNX0c1THJPLTlhNlZASbzJHVGdDNXFKYTNHTHQ1QU9WQlJ5VlVZAVC14V28ZD",
            },
          }
        );
        this.posts = response.data.data;
      } catch (error) {
        console.error("Error fetching user posts:", error);
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
.post-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-gap: 20px;
}

.post-card {
  display: flex;
  flex-direction: column;
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
}

.post-image img {
  width: 100%;
  height: auto;
  object-fit: cover;
}

.post-title {
  padding: 10px;
  background-color: #f7f7f7;
  text-align: center;
}

.loading {
  width: 100%;
  text-align: center;
  margin-top: 20px;
  font-style: italic;
}
</style>
