<template>
  <template v-if="!loading && items.length > 0">
    <Stories :interval="interval" :stories="items" />
  </template>
</template>

<script>
import { Stories } from "vue-insta-stories";

export default {
  data() {
    return {
      loading: true,
      error: null,
      interval: 5000,
      items: [],
    };
  },
  components: { Stories },
  mounted() {
    fetch("https://api.jerem.cool/posts?tags=photography")
      .then((response) => {
        if (!response.ok) {
          throw new Error("Network response was not ok " + response.statusText);
        }
        return response.json();
      })
      .then((data) => {
        this.items = data.items.sort((a, b) => {
          return new Date(b.created_at) - new Date(a.created_at);
        });
        this.loading = false;
      })
      .catch((error) => {
        this.error = "Failed to load posts: " + error.message;
        this.loading = false;
      });
  },
};
</script>

<style>
.vue-insta-stories {
  position: absolute;
  height: 100vh;
  height: -webkit-fill-available;
  width: 100vw;
  top: 0;
}

@media (min-width: 768px) {
  .vue-insta-stories {
    position: relative;
    height: 730px;
    width: 420px;
  }
}
</style>
