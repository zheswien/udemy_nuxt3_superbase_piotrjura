<template>
  <div>Project Lists</div>

  <section v-if="pending">Loading...</section>
  <section v-else-if="error">Error: {{ error.message }}</section>
  <section v-else>
    <ul class="grid grid-cols-1 gap-4">
      <li
        v-for="repo in repos"
        :key="repo.id"
        class="border border-gray-200 p-4 rounded-sm hover:bg-gray-100 font-mono"
      >
        <a :href="repo.html_url" target="_blank">
          <div class="flex justify-between items-center text-sm">
            <div class="font-semibold">
              {{ repo.name }}
            </div>
            <div>{{ repo.stargazers_count }}⭐</div>
          </div>
          <p class="text-sm">{{ repo.description }}</p>
        </a>
      </li>
    </ul>
  </section>
</template>

<script setup>
const { error, pending, data } = await useFetch(
  "https://api.github.com/users/piotr-jura-udemy/repos"
);

const repos = computed(() =>
  // Only filter repos with a description
  data.value
    .filter((repo) => repo.description)
    .sort((a, b) => b.stargazers_count - a.stargazers_count)
);
</script>
