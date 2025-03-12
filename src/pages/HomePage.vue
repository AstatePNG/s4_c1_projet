<script setup>
import { ref, computed } from "vue";
import PostCard from "@/components/PostCard.vue";
import { ChevronDoubleRightIcon } from "@heroicons/vue/24/outline";

const text = ref("");
const trimmedText = computed(() => text.value.trim());

const posts = ref([]);
const sortedPosts = computed(() =>
  posts.value.toSorted((postA, postB) => postB.createdAt - postA.createdAt),
);

function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2),
    content: trimmedText.value,
    createdAt: new Date(),
    liked: false,
    author: {
      id: Math.random().toString(36).substring(2),
      username: "Larry",
      avatarUrl:
        "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT8J5UjvXPi7QQ_reilE_xbSlKTYK5Q0ncI8w&s",
    },
  };

  posts.value.unshift(newPost);
  text.value = "";

  setTimeout(() => {
    const postElement = document.querySelector("article:first-of-type");
    if (postElement) {
      postElement.style.transition = "transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1)";
      postElement.style.transform = "scale(0.4)";

      setTimeout(() => {
        postElement.style.transform = "scale(1)";
      }, 100);
    }
  }, 0);
}

function deletePost(postId) {
  posts.value = posts.value.filter((post) => post.id !== postId);
}

function likePost(postId) {
  const postToUpdate = posts.value.find((post) => post.id === postId);
  postToUpdate.liked = !postToUpdate.liked;
}

function respondToPost(postId) {
  const post = posts.value.find((post) => post.id === postId);
}

const loading = ref(false);
const apiPosts = ref([]);

function fetchPosts() {
  loading.value = true;
  const result = fetch("https://posts-crud-api.vercel.app/posts");
  result.then((response) => response.json()).then((data) => {
    apiPosts.value = data;
    loading.value = false;
  });
}

fetchPosts();
</script>

<template>
  <main>
    <div class="container">
      <h1>Fil d'actualité</h1>
      <form class="card" @submit.prevent="addPost">
        <textarea
          name="post"
          id="post"
          placeholder="Quelles sont les nouvelles à la cour mon brave ?"
          maxlength="200"
          v-model="text"
        ></textarea>
      <button class="btn-icon" :disabled="!trimmedText" type="submit">
        <ChevronDoubleRightIcon />
      </button>
      </form>

      <p v-if="loading">Chargement...</p>

      <p v-else-if="!apiPosts.length">Pas de post pour le moment.</p>

      <PostCard
        v-for="(post, index) in apiPosts"
        :key="index"
        :post="post"
        @delete="deletePost"
        @answer="respondToPost"
        @like="likePost"
      />
    </div>
  </main>
</template>

