<template>
  <template v-if="!loading && items.length > 0">
    <Stories
      v-model:currentIndex="currentIndex"
      :interval="interval"
      :stories="items"
      :isPaused="false"
      :class="{
        'bg-black': currentIndex !== 0,
        'bg-green': currentIndex === 0,
      }"
    >
      <template #intro="story">
        <intro-slide></intro-slide>
      </template>
      <template #title="story">
        <title-slide :story="story"></title-slide>
      </template>
    </Stories>
  </template>
</template>

<script>
import { Stories } from "vue-insta-stories";

import titleSlide from "./components/titleSlide.vue";
import introSlide from "./components/introSlide.vue";

export default {
  data() {
    return {
      loading: true,
      error: null,
      interval: 3000,
      currentIndex: 0,
      items: [
        {
          template: "intro",
        },
      ],
    };
  },
  components: { Stories, titleSlide, introSlide },
  watch: {
    currentIndex(newIndex) {
      console.log("Current index updated:", newIndex);
    },
  },
  mounted() {
    fetch("https://api.jerem.cool/posts?tags=bizikleta")
      .then((response) => {
        if (!response.ok) {
          throw new Error("Network response was not ok " + response.statusText);
        }
        return response.json();
      })
      .then((data) => {
        const dateSet = new Set();

        data.items.forEach((item) => {
          const date = item.created_at.split("T")[0];
          if (!dateSet.has(date) && item.type === "image") {
            dateSet.add(date);
            this.items.push({
              template: "title",
              content: item.content,
              url: item.url,
              type: "content",
              created_at: item.created_at,
            });
          }
          if (item.type === "image") {
            this.items.push(item);
          }
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

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Neucha&display=swap");

* {
  box-sizing: inherit;
  &:before,
  &:after {
    box-sizing: inherit;
  }
}

html {
  font-family: "Neucha", cursive;
  font-weight: 400;
  font-style: normal;
  color: white;
}

#app {
  display: flex;
  background: #89a396;
  width: 100vw;
  height: 100vh;
}

.vue-insta-stories {
  position: absolute;
  height: 100vh;
  height: -webkit-fill-available;
  width: 100vw;
  top: 0;
  background: #5f7d73 !important;

  .timeline {
    background: none !important;
  }

  &.bg-green {
    background: #5f7d73 !important;
  }

  &.bg-black {
    background: #5f7d73 !important;
  }
}

@media (min-width: 768px) {
  h1 {
    display: block;
  }

  .vue-insta-stories {
    position: relative;
    height: 730px;
    width: 420px;
    margin: auto;
    border-radius: 25px;
    overflow: hidden;
    padding: 20px 10px;
    border: 2px solid white;
    box-shadow: rgb(38, 57, 77) 0px 20px 30px -10px;

    .timeline {
      width: calc(100% - 20px) !important;
    }

    &.bg-green {
      background: #5f7d73 !important;
    }

    &.bg-black {
      background: black !important;
    }
  }
}
</style>
