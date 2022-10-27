<template>
  <div>
    <SignBoard :width="500" :height="300" @save="saveImage" />
    <img v-if="imgSrc" :src="imgSrc" alt="signature image" />
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      imgSrc: null,
    }
  },
  methods: {
    saveImage(img) {
      this.imgSrc = img
    },
  },
}
</script>
