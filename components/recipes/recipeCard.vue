<script setup lang="ts">
  const { recipe } = defineProps<{
    recipe: {
      id: string;
      name: string;
      image: string;
      tags: string[];
    };
  }>();
</script>

<template>
  <NuxtLink
    :to="{ name: 'recipe', params: { id: recipe.id } }"
    :style="{
      backgroundImage: `url(${recipe.image})`,
    }"
    class="relative flex h-80 max-w-80 items-end rounded bg-slate-950 bg-cover p-6"
  >
    <div class="absolute inset-0 bg-gradient-to-t from-slate-950 to-transparent" />
    <div class="z-10">
      <h5 class="mb-2 text-xl font-bold tracking-tight">
        {{ recipe.name }}
      </h5>
      <div class="flex flex-wrap gap-1">
        <span
          v-for="tag in recipe.tags"
          :key="tag"
          class="me-2 rounded bg-slate-100 px-2.5 py-0.5 text-xs font-medium text-slate-950"
          >{{ tag }}</span
        >
      </div>
    </div>
  </NuxtLink>
</template>
