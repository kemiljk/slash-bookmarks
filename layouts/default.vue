<template>
  <div class="pb-16 h-full w-full">
    <Nuxt />
  </div>
</template>

<script>
export default {
  head() {
    return {
      bodyAttrs: {
        class: "bg-gray-50 dark:bg-black",
      },
    };
  },
};
</script>
