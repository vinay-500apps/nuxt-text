<template>
  <div>
    <!-- sssEdit -->
  </div>
</template>
<script setup lang="ts">
const { $bus } = useNuxtApp();
onMounted(() => {
  $bus.$on("mitttttttttt", () => {
    console.log("mitttttttttt");
  });
});
</script>
