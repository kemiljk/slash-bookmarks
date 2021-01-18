<template>
  <a
    :href="bookmark.metadata.url"
    target="`_blank"
    rel="noreferrer"
    class="cursor-pointer mb-0"
  >
    <button class="text-left w-full focus:outline-none focus:ring-2 focus:ring-white">
      <header class="flex items-top justify-between pt-4 pb-2">
        <h1
          class="text-red-600 dark:text-red-400 text-lg font-bold w-60 md:w-80"
        >
          {{ bookmark.title }}
        </h1>
        <bookmark-icon class="text-gray-700 dark:text-gray-300"></bookmark-icon>
      </header>
      <p
        class="font-mono text-gray-500 dark:text-gray-400 pt-0 pb-2 font-medium"
      >
        {{ bookmark.created_at | moment("from", "now") }}
      </p>
      <p
        class="text-gray-700 dark:text-gray-300 pb-4 font-medium mb-0"
        v-html="bookmark.metadata.snippet"
      >
        {{ bookmark.metadata.snippet }}
      </p>
    </button>
  </a>
</template>

<script>
import { BookmarkIcon } from "vue-feather-icons";

export default {
  name: "Bookmark",
  components: {
    BookmarkIcon,
  },
  props: {
    bookmark: {
      type: Object,
      default: () => {
        "No links are loaded";
      },
    },
  },
};
</script>
