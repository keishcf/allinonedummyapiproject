<script setup lang="ts">
  import { ref, watch } from "vue";
  import { useRoute, useRouter } from "vue-router";

  export interface Recipe {
    id: number;
    name: string;
    ingredients: string[];
    instructions: string[];
    prepTimeMinutes: number;
    cookTimeMinutes: number;
    servings: number;
    difficulty: string;
    cuisine: string;
    caloriesPerServing: number;
    tags: string[];
    userId: number;
    image: string;
    rating: number;
    reviewCount: number;
    mealType: string[];
  }

  export interface RecipeResponse {
    recipes: Recipe[];
    total: number;
    skip: number;
    limit: number;
  }

  const route = useRoute();
  const router = useRouter();
  const recipeUrl = ref("https://dummyjson.com/recipes?limit=12");

  const recipeSearchQuery = ref("");
  const recipesData: RecipeResponse = ref(null);
  const recipeStatus = ref(null);
  const recipeError = ref(null);

  const sortItem = ref("name");
  const sortItems = ["name", "date"];

  const fetchRecipes = async (url: string) => {
    recipeStatus.value = "pending";
    try {
      const data = await $fetch(url);
      recipesData.value = data;
      recipeStatus.value = "success";
      recipeError.value = null;
    } catch (error) {
      recipeError.value = error;
      recipeStatus.value = "error";
    }
  };

  // Initial fetch
  fetchRecipes(recipeUrl.value);

  watch(recipeSearchQuery, (newQuery) => {
    const searchUrl = `https://dummyjson.com/recipes/search?q=${newQuery}`;
    fetchRecipes(searchUrl);
  });

  watch(sortItem, (newQuery, oldQuery) => {
    if (newQuery === "name" || oldQuery === "name") {
      const searchUrl = `https://dummyjson.com/recipes?limit=12&sortBy=${newQuery}&order=asc`;
      fetchRecipes(searchUrl);
    }
  });

  const updateQueryParam = () => {
    if (recipeSearchQuery.value) {
      router.push({
        path: route.path,
        query: { ...route.query, recipename: recipeSearchQuery.value },
      });
    } else {
      const { recipename, ...otherQueries } = route.query;
      router.push({
        path: route.path,
      });
    }
  };
</script>

<template>
  <div>
    <div class="mb-5">
      <form class="grid grid-cols-6 gap-5">
        <div class="col-span-5">
          <label for="search" class="sr-only mb-2 text-sm font-medium text-gray-900 dark:text-white"
            >Search</label
          >
          <div class="relative">
            <div class="pointer-events-none absolute inset-y-0 start-0 flex items-center ps-3">
              <svg
                class="h-4 w-4 text-gray-500 dark:text-gray-400"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 20 20"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"
                />
              </svg>
            </div>
            <input
              id="search"
              v-model="recipeSearchQuery"
              type="search"
              class="block w-full rounded-lg border border-white bg-slate-950 p-2.5 ps-10 text-sm text-white focus:border-blue-500 focus:ring-blue-500"
              placeholder="Search"
              required
              @input="updateQueryParam"
            />
          </div>
        </div>
        <div class="col-span-1">
          <UiDropdownMenu>
            <UiDropdownMenuTrigger class="w-full"
              ><UiButton class="w-full bg-white text-slate-950 hover:bg-slate-300"
                >Sort</UiButton
              ></UiDropdownMenuTrigger
            >
            <UiDropdownMenuContent
              ><UiDropdownMenuLabel
                label="Sort Recipes" /><UiDropdownMenuSeparator /><UiDropdownMenuRadioGroup
                v-model="sortItem"
              >
                <UiDropdownMenuRadioItem
                  v-for="item in sortItems"
                  :key="item"
                  :value="item"
                  :title="item"
                  :text-value="item"
                /> </UiDropdownMenuRadioGroup></UiDropdownMenuContent
          ></UiDropdownMenu>
        </div>
        <!-- <div class="col-span-2">huin</div> -->
      </form>
    </div>

    <div class="mb-5 flex gap-5">
      <span class="me-2 rounded bg-white px-2.5 py-0.5 text-xs font-bold text-blue-800"
        >Default</span
      >
    </div>

    <div v-if="recipeStatus === 'pending'" class="animate-pulse">
      <div class="grid grid-cols-2 gap-5 sm:grid-cols-6">
        <div
          v-for="card in 12"
          :key="card"
          class="relative flex h-80 max-w-80 animate-pulse items-end rounded bg-slate-950 bg-cover p-6"
        >
          <div class="absolute inset-0 bg-gradient-to-t from-slate-900 to-slate-800" />
          <div class="z-10 w-full">
            <div class="mb-2 h-6 w-3/4 rounded bg-slate-700" />
            <div class="flex flex-wrap gap-1">
              <span
                class="me-2 w-1/4 rounded bg-slate-700 px-2.5 py-0.5 text-xs font-medium text-slate-700"
              />
              <span
                class="me-2 w-1/4 rounded bg-slate-700 px-2.5 py-0.5 text-xs font-medium text-slate-700"
              />
              <span
                class="me-2 w-1/4 rounded bg-slate-700 px-2.5 py-0.5 text-xs font-medium text-slate-700"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else-if="recipesData">
      <div class="grid grid-cols-2 gap-5 sm:grid-cols-6">
        <RecipesRecipeCard
          v-for="recipe in recipesData.recipes"
          :key="recipe.id"
          :recipe="recipe"
        />
      </div>
    </div>
    <div v-if="recipeError">
      <div class="block max-w-sm rounded border border-slate-600 bg-slate-950 p-6">
        <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Error</h5>
        <p class="font-normal text-gray-700 dark:text-gray-400">
          {{ recipeError }}
        </p>
      </div>
    </div>
    <UiPagination
      :total="recipesData?.total"
      :sibling-count="1"
      :items-per-page="recipesData?.recipes?.length"
    />
  </div>
</template>
