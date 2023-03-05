<script setup>
const year = new Date().getFullYear();
</script>

<template>
  <main
    role="main"
    class="sm:max-w-xl px-10 md=max-w-2xl lg:max-w-4xl xl:max-w-6xl py-10 min-h-screen mx-auto w-full flex flex-col justify-between items-center"
  >
    <div class="flex flex-col items-center">
      <h1 class="text-5xl font-bold">Nicolas Bordeaux</h1>
      <h3 class="text-xl font-light text-gray-500">Fullstack developer</h3>
    </div>
    <slot />
    <footer>
      <p>© {{ year }} Nicolas Bordeaux</p>
    </footer>
  </main>
</template>
