<script setup lang="ts">
  definePageMeta({
    layout: "index-layout",
  });
</script>
<template>
  <div>
    <div class="flex min-h-screen items-center justify-center">
      <div class="p-10 text-center">
        <h1
          class="mb-8 max-w-5xl bg-gradient-to-r from-red-600 via-purple-600 to-yellow-600 bg-clip-text text-7xl font-bold text-transparent"
        >
          Welcome to My Vue Frontend Exercise Project
        </h1>
        <p class="mb-6 text-lg">
          This project will utilize the
          <NuxtLink to="https://dummyjson.com/docs" class="font-bold underline"
            >DummyJSON API</NuxtLink
          >
          to create a full-featured web app with excellent SEO metrics and engaging content.
        </p>
        <p class="text-md mb-4 italic text-slate-300">
          Stay tuned for an exciting journey into the world of Vue.js and modern web development!
        </p>
      </div>
    </div>
  </div>
</template>
