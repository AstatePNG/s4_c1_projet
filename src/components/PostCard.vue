<script setup>
import { TrashIcon } from "@heroicons/vue/24/outline";
import { HandThumbUpIcon as likeIcon } from "@heroicons/vue/24/outline";
import { HandThumbUpIcon as likedIcon } from "@heroicons/vue/24/solid";

defineProps(["post"]);
const emit = defineEmits(["delete", "answer", "like"]);
</script>

<template>
  <article class="card">
    <header class="author">
      <img :src="post.author.avatarUrl" alt="avatar" id="avatar" />
      <p>{{ post.author.username }}</p>
    </header>
    <div class="content">
      <p>{{ post.post.content }}</p>
      <p>{{ post.post.createdAt.toLocaleString() }}</p>
    </div>

    <footer class="reactions">
      <button @click="emit('answer', post.post.id)">Répondre</button>
      <button class="btn-icon delete" @click="emit('delete', post.post.id)">
        <TrashIcon />
      </button>
      <button v-if="!post.liked" class="btn-icon like" @click="emit('like', post.post.id)">
        <likeIcon />
      </button>
      <button v-else class="btn-icon like liked" @click="emit('like', post.post.id)">
        <likedIcon />
      </button>
    </footer>
  </article>
</template>
