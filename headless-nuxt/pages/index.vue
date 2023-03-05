<script setup>
const { find } = useStrapi();
const projects = ref();
const types = ref([]);
const activeFilter = ref("all");

const filterProjects = (type) => {
  activeFilter.value = type;
};

const filteredProjects = computed(() => {
  if (activeFilter.value === "all") {
    return projects.value.data;
  }
  return projects.value.data.filter(
    (project) => project.type === activeFilter.value
  );
});

onMounted(async () => {
  projects.value = await find("projects", {
    populate: "deep",
    sort: "id:asc",
  });
  types.value = new Set(projects.value.data.map((project) => project.type));
});
</script>

<template>
  <div v-if="types" class="my-8">
    <button
      :class="{
        'bg-indigo-500 text-white': activeFilter === 'all',
        'bg-white text-indigo-500': activeFilter !== 'all',
      }"
      class="mb-4 mr-4 px-4 py-2 rounded-lg text-xl font-medium hover:bg-indigo-500 hover:text-white"
      @click="filterProjects('all')"
    >
      all
    </button>
    <button
      v-for="type in types"
      :key="type"
      @click="filterProjects(type)"
      :class="{
        'bg-indigo-500 text-white': activeFilter === type,
        'bg-white text-indigo-500': activeFilter !== type,
      }"
      class="mb-4 mr-4 px-4 py-2 rounded-lg text-xl font-medium hover:bg-indigo-500 hover:text-white"
    >
      {{ type }}
    </button>
  </div>
  <div v-if="projects" class="flex flex-wrap justify-center container">
    <nuxt-link
      :to="`/projects/${project.slug}`"
      v-for="project in filteredProjects"
      :key="project.slug"
    >
      <div
        class="flex flex-col items-center m-6 p-6 drop-shadow-2xl bg-[#f5eae1] transform hover:scale-105 transition duration-300 ease-in-out"
      >
        <p class="text-2xl mb-4">{{ project.name }}</p>
        <img class="w-80" :src="project.image.url" alt="" />
      </div>
    </nuxt-link>
  </div>
</template>
