<script setup lang="ts">
  const message = ref<string>('Hello, Vue 3.2.0!')
</script>

<template>
  <div>
    <h1>{{ message }}</h1>
    <NuxtLayout >
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
