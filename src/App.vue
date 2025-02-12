<script setup>
import { ref } from "vue";

let text = ref(""); // ref permet à la variable de se mettre automatiquement à jour dans le HTML

function updateText(event) {
  text.value = event.target.value;
}
</script>

<template>
  <main>
    <div class="container">
      <form class="card">
        <textarea
          name="post"
          id="post"
          placeholder="Quelles sont les dernières nouvelles mon brave ?"
          v-bind:value="text"
          v-on:input="updateText"
        >
          <!-- v-bind permet la liaison entre la valeur de la variable js et la variable html, de js vers html -->
          <!-- input appelle une fonction à chaque input (observateur), cela permet la liaison de html vers js -->
          <!-- pour que la chaine de caractères soit considérer comme un nom js, on lui ajoute v-on -->
        </textarea>
        <button type="submit">>>></button>
      </form>

      <p>
        {{ text }}
        <!-- les doubles accolades indiquent l'utilisation de la valeur de la variable js "text"-->
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
