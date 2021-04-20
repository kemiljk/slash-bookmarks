<template>
  <div class="border-b-2 border-gray-200 dark:border-gray-700">
    <a
      :href="bookmark.metadata.url"
      target="_blank"
      rel="noreferrer"
      class="cursor-pointer mb-0"
      @click="markAsRead()"
    >
      <button
        class="text-left w-full focus:outline-none focus:ring-2 focus:ring-white"
        type="button"
        :class="[bookmark.metadata.is_read ? 'opacity-50' : 'opacity-100']"
      >
        <header class="flex items-top justify-between pt-4 pb-2">
          <h1 class="text-black dark:text-white text-xl font-bold">
            {{ bookmark.title }}
          </h1>
          <check-icon
            :class="[
              bookmark.metadata.is_read
                ? 'text-green-700 dark:text-green-400 flex-shrink-0 ml-4'
                : 'hidden',
            ]"
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
    <div class="flex space-x-2 py-6 justify-between">
      <div flex space-x-2>
        <button
          type="button"
          class="bg-gray-200 dark:bg-gray-800 hover:bg-gray-500 dark:hover:bg-gray-700 text-white rounded-full px-3 py-3"
          @click="markAsUnread()"
        >
          <div class="flex justify-between items-center">
            <book-open-icon
              size="1x"
              class="text-gray-700 dark:text-gray-300"
            />
          </div>
        </button>
        <button
          type="button"
          class="bg-red-400 dark:bg-red-900 hover:bg-red-500 dark:hover:bg-red-800 text-white rounded-full px-3 py-3"
          @click="deleteBookmark()"
        >
          <div class="flex justify-between items-center">
            <trash-2-icon size="1x" class="text-white dark:text-white" />
          </div>
        </button>
      </div>
      <button
        type="button"
        class="bg-gray-200 dark:bg-gray-800 hover:bg-gray-300 dark:hover:bg-gray-700 text-white rounded-full px-3 py-3"
        @click="
          copyToClipboard();
          show = true;
        "
      >
        <div class="flex justify-between items-center">
          <check-icon
            v-show="show"
            size="1x"
            class="text-green-500 dark:text-green-300"
          />
          <link-2-icon
            v-show="!show"
            size="1x"
            class="text-gray-700 dark:text-gray-300"
          />
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
  Link2Icon,
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
    Link2Icon,
  },
  data() {
    return {
      show: false,
    };
  },
  methods: {
    markAsRead() {
      this.bookmark.metadata.is_read = true;
      this.$emit("markAsRead", this.bookmark.slug);
    },
    markAsUnread() {
      this.bookmark.metadata.is_read = false;
      this.$emit("markAsUnread", this.bookmark.slug);
    },
    deleteBookmark() {
      this.$emit("deleteBookmark", this.bookmark.id);
    },
    copyToClipboard() {
      setTimeout(() => (this.show = false), 2000);
      this.$emit("copyToClipboard", this.bookmark.metadata.url);
    },
  },
};
</script>
