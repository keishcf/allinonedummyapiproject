<template>
  <div class="flex items-center justify-center">
    <div class="container mx-auto">
      <div>
        <nav class="flex justify-center gap-12 py-5">
          <NuxtLink :to="{ name: 'recipes' }" class="font-bold hover:underline">Recipes</NuxtLink>
          <a href="#" class="font-bold hover:underline">Blog</a>
          <a href="#" class="font-bold hover:underline">Todos</a>
          <a href="#" class="font-bold hover:underline">Qoute</a>
          <a href="#" class="font-bold hover:underline">Users</a>
          <a href="#" class="font-bold hover:underline">Products</a>
        </nav>
      </div>
      <slot />
    </div>
  </div>
</template>
