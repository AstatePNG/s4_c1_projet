<script setup>
import { useRoute } from 'vue-router';

const route = useRoute();

const username = route.params.username;

const title = "Profil de " + username;
</script>

<template>
  <h1>{{ title }}</h1>
  <h2>{{ $route.params.username }}</h2>
</template>
