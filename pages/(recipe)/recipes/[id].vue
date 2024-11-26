<script setup lang="ts">
  const route = useRoute();
  const {
    data: recipeData,
    status: recipeStatus,
    error: recipeError,
  } = await useFetch(`https://dummyjson.com/recipes/${route.params.id}`);

  useSeoMeta({
    title: recipeData.value.name,
  });
  definePageMeta({
    name: "recipe",
  });
</script>
<template>
  <div class="grid grid-cols-7 gap-x-16">
    <div class="col-span-4">
      <h1 class="mb-5 text-6xl font-bold tracking-wide">
        {{ recipeData.name }}
      </h1>
      <div class="mb-4 flex gap-2">
        <p class="text-slate-400">
          Prepared in
          <span class="font-bold text-white">{{ recipeData.prepTimeMinutes }} Mins</span>
        </p>
        <p class="text-slate-400">
          Cooking could take
          <span class="font-bold text-white">{{ recipeData.cookTimeMinutes }} Mins</span>
        </p>

        <Rating :rating="recipeData.rating" :review-count="recipeData.reviewCount" />
      </div>
      <img
        :src="recipeData.image"
        :alt="recipeData.name"
        class="h-2/4 w-full rounded-lg object-cover"
      />
      <div class="pt-5">
        <h2 class="mb-5 text-3xl font-bold tracking-wide">Instructions</h2>
        <p
          v-for="instruction in recipeData.instructions"
          :key="instruction"
          class="font-normal text-slate-200"
        >
          {{ instruction }}
        </p>
      </div>
    </div>
    <div class="col-span-2">
      <div class="border bg-slate-900 p-5">
        <h2 class="mb-5 text-xl font-bold tracking-wide">Ingredients</h2>

        <p
          v-for="ingredient in recipeData.ingredients"
          :key="ingredient"
          class="font-normal text-slate-200"
        >
          {{ ingredient }}
        </p>
      </div>
      <div class="mt-4">
        <h2 class="mb-5 text-xl font-bold tracking-wide">Additional Information</h2>
        <hr class="my-5" />
        <p class="font-normal text-slate-200">
          <span class="font-bold text-white">Serving:</span>
          {{ recipeData.servings }}
        </p>
        <p class="font-normal text-slate-200">
          <span class="font-bold text-white">Difficulty:</span>
          {{ recipeData.difficulty }}
        </p>
        <p class="font-normal text-slate-200">
          <span class="font-bold text-white">Cuisine:</span>
          {{ recipeData.cuisine }}
        </p>
        <p class="font-normal text-slate-200">
          <span class="font-bold text-white">Calories per Serving:</span>
          {{ recipeData.caloriesPerServing }}
        </p>
        <p class="font-normal text-slate-200">
          <span class="font-bold text-white">Tags:</span>
          <span class="px-2 underline" v-for="tag in recipeData.tags" :key="tag">{{ tag }}</span>
        </p>
        <p class="font-normal text-slate-200">
          <span class="font-bold text-white">Meal Type:</span>
          <span v-for="meal in recipeData.mealType" :key="meal">{{ meal }}</span>
        </p>
      </div>
    </div>
  </div>
</template>
