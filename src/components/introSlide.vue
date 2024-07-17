<template>
  <div class="slide-intro">
    <p>
      Coucou 👋 <br /><br />
      Je vais au pays basque en vélo <br />
      <small>(et j'y reste un peu après)</small><br />
      en partant de Bordeaux et voici les photos 👉
    </p>
  </div>
</template>

<script>
export default {
  name: "Story Intro",
};
</script>

<style lang="scss">
.slide-intro {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: flex-end;
  border-radius: 25px;

  p {
    padding: 45px 45px 90px 45px;
    letter-spacing: 1px;
    line-height: 2.2rem;
    font-size: 32px;
  }

  small {
    font-size: 0.5em;
  }
}
</style>
