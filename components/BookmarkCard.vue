<template>
  <div
    class="flex-col w-full bg-white shadow-md hover:shadow-lg transform transition-all dark:shadow-none dark:bg-gray-900 rounded-3xl"
  >
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
        :class="[bookmark.metadata.read ? 'opacity-50' : 'opacity-100']"
      >
        <div class="pt-4 px-4">
          <header class="flex items-top justify-between pb-2">
            <h1 class="text-black dark:text-white text-xl font-bold">
              {{ bookmark.title }}
            </h1>
            <check-icon
              :class="[
                bookmark.metadata.read
                  ? 'text-green-700 dark:text-green-400 flex-shrink-0 ml-4'
                  : 'hidden',
              ]"
            />
          </header>
          <p
            class="text-gray-700 dark:text-gray-300 pt-2 text-sm"
            v-html="bookmark.metadata.snippet"
          >
            {{ bookmark.metadata.snippet }}
          </p>
        </div>
      </button>
    </a>
    <!-- Action items -->
    <div
      class="flex mt-6 pt-4 justify-between border-t border-gray-100 dark:border-gray-700 pb-4 px-4"
    >
    <p class="font-mono text-gray-500 dark:text-gray-400 p-0"
          >
            {{ bookmark.created_at | moment("from", "now") }}
      </p>
      <button
        type="button"
        class="bg-gray-200 dark:bg-gray-800 hover:bg-gray-300 dark:hover:bg-gray-700 text-white rounded-full px-3 py-2"
        @click="
          copyToClipboard();
          show = true;
        "
      >
        <div class="flex justify-between items-center">
          <check-icon
            v-show="show"
            size="1x"
            class="mr-2 text-green-500 dark:text-green-300"
          />
          <link-2-icon
            v-show="!show"
            size="1x"
            class="mr-2 text-gray-700 dark:text-gray-300"
          />
          <span class="text-xs uppercase text-gray-700 dark:text-gray-300">Copy link</span>
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
      this.bookmark.metadata.read = true;
      this.$emit("markAsRead", this.bookmark.id);
    },
    markAsUnread() {
      this.bookmark.metadata.read = false;
      this.$emit("markAsUnread", this.bookmark.id);
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
