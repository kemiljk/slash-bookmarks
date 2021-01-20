<template>
  <div>
    <a
      :href="bookmark.metadata.url"
      target="`_blank"
      rel="noreferrer"
      class="cursor-pointer mb-0"
      @click="
        markAsRead;
        toggle();
      "
    >
      <button
        class="text-left w-full focus:outline-none focus:ring-2 focus:ring-white"
        type="button"
        :class="[
          bookmark.metadata.is_read || read === true
            ? 'opacity-50'
            : 'opacity-100',
        ]"
      >
        <header class="flex items-top justify-between pt-4 pb-2">
          <h1
            class="'text-red-600 dark:text-red-400 text-lg font-bold w-60 md:w-80"
          >
            {{ bookmark.title }}
          </h1>
          <check-icon
            v-if="bookmark.metadata.is_read || read === true"
            class="text-green-700 dark:text-green-400"
          />
          <bookmark-icon v-else class="text-gray-700 dark:text-gray-300" />
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
    <!-- <div class="flex space-x-4">
      <button type="button" class="bg-gray-200 dark:bg-gray-800 hover:bg-gray-500 dark:hover:bg-gray-700 text-white rounded-full px-4 py-2">
        <div class="flex justify-between items-center">
          <book-open-icon size="1x" class="mr-2 text-gray-700 dark:text-gray-300" />
          <span class="text-gray-700 dark:text-gray-300">Unread</span>
        </div>
      </button>
      <button type="button"  class="bg-gray-200 dark:bg-gray-800 hover:bg-gray-500 dark:hover:bg-gray-700 text-white rounded-full px-4 py-2">
        <div class="flex justify-between items-center">
          <trash-2-icon size="1x" class="mr-2 text-gray-700 dark:text-gray-300" />
          <span class="text-gray-700 dark:text-gray-300">Delete</span>
        </div>
      </button>
    </div> -->
  </div>
</template>

<script>
import {
  BookmarkIcon,
  CheckIcon,
  BookOpenIcon,
  Trash2Icon,
} from "vue-feather-icons";

export default {
  name: "Bookmark",
  props: {
    bookmark: {
      type: Object,
      default: () => {
        "No links are loaded";
      },
    },
  },
  components: {
    BookmarkIcon,
    CheckIcon,
    BookOpenIcon,
    Trash2Icon,
  },
  data() {
    return {
      read: false,
    };
  },
  methods: {
    markAsRead() {
      this.$emit("markAsRead", "wilson-miner-staff-design");
    },
    toggle() {
      this.read = true;
    },
  },
};
</script>
