<script setup>
const { findOne } = useStrapi();
const route = useRoute();
const project = ref();

onMounted(async () => {
  project.value = await findOne(
    `projects?filters[slug]=${route.params.slug}&populate=deep`
  );
  project.value = project.value.data[0];
});
</script>

<template>
  <div v-if="project" class="relative flex flex-col items-center mb-16">
    <span class="absolute top-0 left-0 ml-8">
      <nuxt-link to="/" class="text-indigo-500 hover:text-indigo-600"
        >Back to projects</nuxt-link
      >
    </span>
    <h1 class="text-7xl font-bold my-8">{{ project.name }}</h1>
    <div class="flex flew-wrap mb-8">
      <img
        class="w-20 m-2"
        v-for="(tech, index) in project.technologies"
        :src="tech.image.url"
        :key="index"
      />
    </div>

    <img class="w-4/6 mb-8" :src="project.image.url" />
    <div class="mb-8" v-html="$mdRenderer.render(project.description)"></div>
    <a
      :href="project.link"
      target="_blank"
      rel="noopener noreferrer"
      class="text-white px-4 py-2 rounded-lg text-xl font-medium bg-indigo-500 hover:bg-indigo-600"
    >
      Visit website
    </a>
  </div>
</template>
