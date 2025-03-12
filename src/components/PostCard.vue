<script setup>
import { TrashIcon, ArrowTurnDownRightIcon } from "@heroicons/vue/24/outline";
import { HeartIcon as LikeIcon } from "@heroicons/vue/24/outline";
import { HeartIcon as SolidLikeIcon } from "@heroicons/vue/24/solid";

defineProps(["post"]);
const emit = defineEmits(["delete", "answer", "like"]);
</script>

<template>
  <article class="card">
    <header class="author">
      <img :src="post.author.avatarUrl" alt="avatar" id="avatar" />
      <RouterLink :to="{name: 'user', params: {username: post.author.username}}" class="author">{{ post.author.username }}</RouterLink>
    </header>
    <div class="content">
      <p>{{ post.content }}</p>
      <p>{{ post.createdAt.toLocaleString() }}</p>
    </div>

    <footer class="reactions">
      <button class="btn-icon answer" @click="emit('answer', post.id)">
        <ArrowTurnDownRightIcon />
      </button>
      <button class="btn-icon like" @click="emit('like', post.id)">
        <LikeIcon v-if="!post.liked" />
        <SolidLikeIcon v-else class="active" />
      </button>
      <button class="btn-icon delete" @click="emit('delete', post.id)">
        <TrashIcon />
      </button>
    </footer>
  </article>
</template>
