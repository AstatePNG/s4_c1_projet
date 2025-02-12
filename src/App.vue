<script setup>
import { ref } from "vue";

const text = ref(""); // ref permet à la variable de se mettre automatiquement à jour dans le HTML
const posts = ref([]);

function addPost() {
  posts.value.push(text.value);
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
          placeholder="Quelles sont les dernières nouvelles mon brave ?"
          v-model="text"
        >
        </textarea>
        <button type="submit">>>></button>
      </form>

      <p v-for="(post, index) in posts" :key="index">
        <!-- la directive v-for permet de faire un foreach -->
        <!-- index devient la clé correspondant à chaque post de façon unique -->
        {{ post }}
      </p>
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
</style>
