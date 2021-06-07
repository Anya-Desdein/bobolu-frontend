<template>
  <div class="container">
    <div v-if="pic.imageDetailInfo">
      <NuxtLink to="/"> Homepage </NuxtLink>
      <h1 class="title">
        {{ pic.imageBasicInfo.name }}
      </h1>
      <a :href="$config.apiBaseURL + pic.imageDetailInfo.imageUrl" target="_blank">
        <img :src="$config.apiBaseURL + pic.imageDetailInfo.imageUrl" :alt="pic.name" />
      </a>
      <p>
        {{ pic.imageDetailInfo.description }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pic: {},
    };
  },
  async fetch() {
    const { id } = this.$route.params;
    const url = `${this.$config.apiBaseURL}/api/pic/${id}`;
    this.pic = await this.$axios.$get(url);
  }
}
</script>

<style scoped>

img {
  max-width: 90vw;
  max-height: 70vh;
  object-fit: contain;
  display: block;
  margin: 1em auto;
}

</style>
