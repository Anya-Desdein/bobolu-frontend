<template>
  <div>
    <NuxtLink v-for="pic in pics" :key="pic.id" :to="`/pic/${pic.id}`">
      <img :src="$config.apiBaseURL + pic.thumbnailUrl" :alt="pic.name" />
    </NuxtLink>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pics: {},
    };
  },
  async fetch() {
    const url = `${this.$config.apiBaseURL}/api/all-pics`;
    const { items } = await this.$axios.$get(url); 
    this.pics = items;
  },
};
</script>

<style scoped>

a {
  display: inline-block;
  margin: 1em;
  width: 12em;
  height: 12em;
  transition: 0.15s transform ease-out;
}

a:hover {
  transform: scale(1.1);
}

a img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

</style>
