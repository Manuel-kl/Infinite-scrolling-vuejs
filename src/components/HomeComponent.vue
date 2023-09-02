<template>
  <div class="home">
    <nav>
      <h1>Vue Infinite Scroll</h1>
    </nav>
    <body>
      <div class="post" id="posts" v-for="post in posts" :key="post.index">
        <div class="name">
          <vue-logo />
          <h3>{{ post.name }}</h3>
        </div>
        <div class="description">
          <p>{{ post.post }}</p>
        </div>
      </div>
      <img
        v-show="loading"
        src="../assets/loader.gif"
        alt="loading-gif"
        class="loading-gif"
      />
    </body>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from "vue";
import VueLogo from "../assets/vue-logo.vue";
import postsJson from "../utils/random.json";

const allPosts = ref([]);
const posts = ref([]);
const loading = ref(false);
const itemsPerPage = 20;

allPosts.value = postsJson.posts.sort(() => Math.random() - 0.5);

const getPosts = () => {
  loading.value = true;
  setTimeout(() => {
    posts.value = allPosts.value.slice(0, itemsPerPage);
    loading.value = false;
  }, 2000);
};

const loadMorePosts = () => {
  loading.value = true;
  setTimeout(() => {
    const remainingPosts = allPosts.value.slice(posts.value.length);
    const newPosts = remainingPosts.slice(0, itemsPerPage);

    // Add the new posts to the existing 'posts' array
    posts.value = [...posts.value, ...newPosts];

    loading.value = false;
  }, 2000);
};

const handleScroll = () => {
  const scrollY = window.scrollY;
  const windowHeight = window.innerHeight;
  const pageHeight = document.documentElement.scrollHeight;

  if (scrollY + windowHeight >= pageHeight - 100 && !loading.value) {
    loadMorePosts();
  }
};

onMounted(() => {
  document.addEventListener("scroll", handleScroll);
  getPosts();
});

onUnmounted(() => {
  document.removeEventListener("scroll", handleScroll);
});
</script>

<style lang="scss">
.home {
  display: flex;
  flex-direction: column;
  align-items: center;

  nav {
    width: 100%;
    color: black;
    background-color: #b39cd0;
    display: flex;
    justify-content: flex-start;
    align-items: left;

    h1 {
      font-size: 2rem;
      padding: 1rem;
    }
  }

  body {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    background-color: rgb(243, 241, 241);
    padding: 1rem;
    gap: 1rem;

    .post {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
      padding: 1rem;
      border-radius: 11px;

      @media (min-width: 400px) {
        width: 350px;
      }

      @media (min-width: 600px) {
        width: 450px;
        // width: 600px;
      }

      @media (min-width: 768px) {
        width: 600px;
      }

      .name {
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: row;

        h3 {
          font-size: 1.5rem;
          padding: 1rem;
        }
      }

      .description {
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: row;

        p {
          font-size: 1rem;
          padding: 1rem;
        }
      }
    }

    img.loading-gif {
      width: 20px;
    }
  }
}
</style>