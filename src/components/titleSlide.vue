<template>
  <div class="slide-title">
    <div class="content">
      <img :src="story.props.story.url" :alt="story.props.story.content" />
      <div class="overlay">
        <p>
          {{ story.props.story.content }}
        </p>
      </div>
    </div>
    <!-- <div>{{ story.props.story.content }} {{ story.props.story.url }}</div> -->
  </div>
</template>

<script>
export default {
  name: "Story Title",
  props: {
    story: {
      type: Object,
    },
  },
  computed: {
    styleObject() {
      return {
        backgroundImage: `url(${this.story.props.story.url})`,
      };
    },
  },
};
</script>

<style lang="scss">
.slide-title {
  position: relative;
  display: flex;
  width: 100%;
  height: 100%;

  .content {
    position: relative;
    margin: auto;
  }

  img {
    display: block;
    width: 100%;
    margin: auto;
  }

  .overlay {
    display: flex;
    align-items: flex-end;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    z-index: 10;
    background: rgba(0, 0, 0, 0.2);
    padding: 20px;

    p {
      font-size: 24px;
      letter-spacing: 1px;
    }
  }
}
</style>
