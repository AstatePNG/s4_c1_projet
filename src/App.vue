<script setup>
import { ref, computed } from "vue";

const text = ref(""); // ref permet à la variable de se mettre automatiquement à jour dans le HTML
const trimmedText = computed(() => text.value.trim()); // computed fait comme ref mais pour les variables nécessitant un traitement

const posts = ref([]);
const sortedPost = computed(() =>
  posts.value.sort(function (a, b) {
    return b.createdAt - a.createdAt;
  }),
);

function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2),
    content: trimmedText.value,
    createdAt: new Date(),
    author: {
      id: Math.random().toString(36).substring(2),
      username: "Larry",
      avatarUrl:
        "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT8J5UjvXPi7QQ_reilE_xbSlKTYK5Q0ncI8w&s",
    },
  };
  posts.value.unshift(newPost);
  text.value = "";
}
</script>

<template>
  <main>
    <div class="container">
      <form class="card" @submit.prevent="addPost">
        <textarea
          name="post"
          id="post"
          maxlength="200"
          placeholder="Quelles sont les dernières nouvelles mon brave ?"
          v-model="text"
        >
        </textarea>
        <button type="submit" :disabled="!trimmedText">>>></button>
      </form>

      <p v-if="!posts.length">Pas de posts pour le moment.</p>

      <article v-for="(post, index) in sortedPost" :key="index" class="card">
        <div class="post-header">
          <img
            :src="post.author.avatarUrl"
            alt="avatar de l'utilisateur"
            width="36"
            height="36"
            class="user-avatar"
          />
          <a>{{ post.author.username }}</a>
        </div>
        <p>
          {{ post.content }}
        </p>
      </article>
    </div>
  </main>
</template>

<style scoped>
.container {
  height: 100vh;
  margin: 0 auto;
  max-width: 640px;
}
.card {
  background-color: var(--color-bg-secondary);
  border-radius: 10px;
  border: 1px solid var(--color-border);
  margin-bottom: 1rem;
}

form {
  display: flex;
  flex-direction: column;
  margin-top: 1rem;
  padding: 1rem 1.5rem;
  width: 100%;
}
textarea {
  background: none;
  border: none;
  color: var(--color-text-primary);
  flex: 1;
  margin-bottom: 1rem;
  outline: none;
  padding: 0.5rem 0;
  resize: none;
  field-sizing: content;
}
button {
  align-self: flex-end;
  background: none;
  border-radius: 10px;
  border: 1px solid var(--color-border);
  color: var(--color-text-primary);
  cursor: pointer;
  font-size: 1rem;
  height: 40px;
  padding: 0 1rem;
}
button:disabled {
  cursor: not-allowed;
  opacity: 0.4;
}

article {
  padding: 0.75rem 1.5rem;
  overflow: hidden;
}

article p {
  white-space: pre-wrap;
}

.user-avatar {
  border-radius: 50%;
  object-fit: cover;
}

.post-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 1rem;
}
</style>
