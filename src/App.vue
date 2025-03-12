<script setup>
import { ref, computed } from "vue";
import PostCard from "@/components/PostCard.vue";

const text = ref("");
const trimmedText = computed(() => text.value.trim());

const posts = ref([]);
const sortedPosts = computed(() =>
  posts.value.toSorted((postA, postB) => postB.createdAt - postA.createdAt),
);

function addPost() {
  const newPost = {
    post: {
      id: Math.random().toString(36).substring(2),
      content: trimmedText.value,
      createdAt: new Date(),
      liked: false,
    },
    author: {
      id: Math.random().toString().substring(2),
      username: "HelloClass",
      avatarUrl: "https://medias.tendanceouest.com/photos/galeries/415231/9052-tab.jpg",
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
  posts.value = posts.value.filter((post) => post.post.id !== postId);
}

function likePost(postId) {
  posts.value.find((post) => post.post.id === postId).liked = !posts.value.find(
    (post) => post.post.id === postId,
  ).liked;
}

function respondToPost(postId) {
  const post = posts.value.find((post) => post.post.id === postId);
}
</script>

<template>
  <main>
    <div class="container">
      <h1>Fil d'actualité</h1>
      <form class="card" @submit.prevent="addPost">
        <textarea
          name="post"
          id="post"
          placeholder="Quoi de neuf ?"
          maxlength="200"
          v-model="text"
        ></textarea>
        <button type="submit" :disabled="!trimmedText">Publier</button>
      </form>
      <p v-if="!posts.length">Pas de post pour le moment.</p>

      <PostCard
        v-for="(post, index) in sortedPosts"
        :key="index"
        :post="post"
        @delete="deletePost"
        @answer="respondToPost"
        @like="likePost"
      />
    </div>
  </main>
</template>

/* function addPost() { const newId = Math.random().toString(36).substring(2); const newPost = {
post: { id: newId, content: trimmedText.value, createdAt: new Date(), }, author: { id:
Math.random().toString().substring(2), username: "HelloClass", avatarUrl:
"https://medias.tendanceouest.com/photos/galeries/415231/9052-tab.jpg", }, };
posts.value.unshift(newPost); text.value = ""; // Add glowing animation setTimeout(() => { const
postElement = document.querySelector('article:first-of-type'); if (postElement) {
postElement.style.transition = 'box-shadow 0.3s ease-in-out'; postElement.style.boxShadow = '0 0
20px rgb(70, 73, 224)'; setTimeout(() => { postElement.style.boxShadow = 'none'; }, 1500); } }, 0);
} */
