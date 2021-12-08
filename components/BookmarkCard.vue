<template>
  <div
    class="bg-white shadow-md hover:shadow-lg transform transition-all dark:shadow-none dark:bg-gray-900 rounded-2xl"
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
        <div class="pt-8 px-8">
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
        </div>
      </button>
    </a>
    <!-- Action items -->
    <div
      class="flex mt-6 pt-4 justify-between border-t border-gray-100 dark:border-gray-700 pb-4 px-8"
    >
      <div class="space-x-4 ">
        <button
          type="button" disabled
          class="opacity-50 bg-gray-200 dark:bg-gray-800 hover:bg-gray-300 dark:hover:bg-gray-700 text-white rounded-full px-3 py-3"
          @click="
            bookmark.metadata.read === true ? markAsUnread() : markAsRead()
          "
        >
          <div class="flex justify-between items-center">
            <book-open-icon
              size="1x"
              class="text-gray-700 dark:text-gray-300"
            />
          </div>
        </button>
        <button
          type="button" disabled
          class="opacity-50 bg-red-400 dark:bg-red-900 hover:bg-red-500 dark:hover:bg-red-800 text-white rounded-full px-3 py-3"
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
