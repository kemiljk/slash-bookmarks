<template>
  <div class="border-b-2 border-gray-200 dark:border-gray-700">
    <a
      :href="bookmark.metadata.url"
      target="_blank"
      rel="noreferrer"
      class="cursor-pointer mb-0"
      @click="
        markAsRead();
        isRead();
      "
    >
      <button
        class="text-left w-full focus:outline-none focus:ring-2 focus:ring-white"
        type="button"
        :class="[
          bookmark.metadata.is_read,
          read === true ? 'opacity-50' : 'opacity-100',
        ]"
      >
        <header class="flex items-top justify-between pt-4 pb-2">
          <h1 class="text-black dark:text-white text-xl font-bold">
            {{ bookmark.title }}
          </h1>
          <check-icon
            v-if="bookmark.metadata.is_read"
            class="text-green-700 dark:text-green-400 flex-shrink-0 ml-4"
          />
          <bookmark-icon
            v-else
            class="text-gray-700 dark:text-yellow-400 flex-shrink-0 ml-4"
          />
        </header>
        <p
          class="font-mono text-gray-500 dark:text-gray-400 pt-0 pb-2 font-medium"
        >
          {{ bookmark.created_at | moment("from", "now") }}
        </p>
        <p
          class="text-gray-700 dark:text-gray-300 pt-4 font-medium"
          v-html="bookmark.metadata.snippet"
        >
          {{ bookmark.metadata.snippet }}
        </p>
      </button>
    </a>
    <div class="flex space-x-2 py-6">
      <button
        type="button"
        class="bg-gray-200 dark:bg-gray-800 hover:bg-gray-500 dark:hover:bg-gray-700 text-white rounded-full px-3 py-1"
        @click="
          markAsUnread();
          isUnread();
        "
      >
        <div class="flex justify-between items-center">
          <book-open-icon
            size="1x"
            class="mr-2 text-gray-700 dark:text-gray-300"
          />
          <span class="text-gray-700 dark:text-gray-300">Unread</span>
        </div>
      </button>
      <button
        type="button"
        class="bg-red-400 dark:bg-red-900 hover:bg-red-500 dark:hover:bg-red-800 text-white rounded-full px-3 py-1"
        @click="deleteBookmark()"
      >
        <div class="flex justify-between items-center">
          <trash-2-icon size="1x" class="mr-2 text-red-700 dark:text-red-100" />
          <span class="text-gray-700 dark:text-red-100">Delete</span>
        </div>
      </button>
    </div>
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
    markAsUnread() {
      this.$emit("markAsUnread", "wilson-miner-staff-design");
    },
    deleteBookmark() {
      this.$emit("deleteBookmark", "");
    },
    isRead() {
      this.read = true;
    },
    isUnread() {
      this.read = false;
    },
  },
};
</script>
