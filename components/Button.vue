<template>
  <button
    class="flex justify-end items-center text-center font-medium rounded-full px-6 py-6 transition duration-500 ease focus:outline-none focus:ring-2 focus:ring-white"
    :class="colorClasses"
    @click="reloadThePage()"
  >
    <slot name="icon" />
  </button>
</template>

<script>
export default {
  props: ["color"],
  computed: {
    colorClasses() {
      return {
        grey:
          "bg-gray-300 dark:bg-gray-700 hover:bg-gray-400 dark:hover:bg-gray-600 text-gray-800 dark:text-white",
      }[this.color];
    },
  },
  methods: {
    reloadThePage() {
      if (process.browser) {
        window.location.reload();
      }
    },
  },
};
</script>
