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
      pics: [
        {
          id: '11',
          name: 'Test image 1!',
          thumbnailUrl: '/img/test_t.png',
        },
        {
          id: '22',
          name: 'Test image 2!',
          thumbnailUrl: '/img/test_t.png',
        },
        {
          id: '33',
          name: 'Test image 3!',
          thumbnailUrl: '/img/test_t.png',
        },
        {
          id: '44',
          name: 'Test image 4!',
          thumbnailUrl: '/img/test_t.png',
        },
      ],
    };
  },
}
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
