<template>
  <div class="relative mx-auto">
    <Nav />
    <div class="max-w-5xl mx-auto px-4 pt-24">
      <div class="w-full grid grid-cols-1 sm:grid-cols-2 gap-4">
        <div v-for="bookmark in bookmarks" :key="bookmark._id">
          <keep-alive>
            <BookmarkCard
              :bookmark="bookmark"
              @markAsRead="markAsRead"
              @markAsUnread="markAsUnread"
              @deleteBookmark="deleteBookmark"
              @copyToClipboard="copyToClipboard"
            />
          </keep-alive>
        </div>
      </div>
      <Button color="grey" type="button" class="fixed bottom-4 right-4">
        <template #icon>
          <refresh-cw-icon size="1x" class="text-gray-700 dark:text-gray-200" />
        </template>
      </Button>
    </div>
  </div>
</template>

<script>
import getSiteMeta from "~/utils/getSiteMeta.js";
import { RefreshCwIcon } from "vue-feather-icons";
import Cosmic from "cosmicjs";
const api = Cosmic();
const bucket = api.bucket({
  slug: process.env.NUXT_ENV_COSMIC_SLUG,
  read_key: process.env.NUXT_ENV_COSMIC_READ_KEY,
  write_key: process.env.NUXT_ENV_COSMIC_WRITE_KEY,
});

export default {
  components: {
    RefreshCwIcon,
  },
  computed: {
    meta() {
      const metaData = {
        title: "KEJK | Bookmarks",
        description:
          "Bookmarks of interesting things I've found around the web, for me to keep and for you to enjoy.",
        url: "https://bookmarks.kejk.tech",
        mainImage:
          "https://res.cloudinary.com/kejk/image/upload/q_auto,f_auto/v1610305389/OG_Image_wlile7.png",
      };
      return getSiteMeta(metaData);
    },
  },
  head() {
    return {
      title: "KEJK | Bookmarks",
      meta: [...this.meta],
      link: [{ rel: "canonical", href: "https://bookmarks.kejk.tech" }],
    };
  },
  data() {
    return {
      bookmarks: {},
      deleting: false,
    };
  },
  created() {
    this.getBookmarksData();
  },
  methods: {
    async getBookmarksData() {
      this.loading = true;
      await bucket
        .getObjects({
          query: {
            type: "bookmarks",
          },
          props: "id,slug,title,metadata,created_at",
          sort: "-created_at",
        })
        .then((data) => {
          const bookmarks = data.objects;
          this.loading = false;
          this.bookmarks = bookmarks;
        });
    },
    markAsRead(object_id) {
      const params = {
        id: `${object_id}`,
        key: `read`,
        value: true,
      };
      bucket
        .editObjectMetafield(params)
        .then((data) => {
          console.log(data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    markAsUnread(object_id) {
      const params = {
        id: `${object_id}`,
        key: `read`,
        value: false,
      };
      bucket
        .editObjectMetafield(params)
        .then((data) => {
          console.log(data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    deleteBookmark(object_id) {
      const params = {
        id: object_id,
        type_slug: "bookmarks",
      };
      this.deleting = true;
      bucket
        .deleteObject(params)
        .then((data) => {
          console.log(data);
          this.bookmarks.splice(object_id, 1);
          this.deleting = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    copyToClipboard(value) {
      navigator.clipboard.writeText(value);
    },
  },
};
</script>
