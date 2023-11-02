<script>
  export default {
    props: {
      maxPages: Number,
    },

    data: () => {
      return {
        currentPage: 1,
      }
    },

    methods: {
      sendPageNum() {
        this.$emit('new-page', this.currentPage);
      }
    }

  }
</script>

<template>
  <div>
    <input
        type="number"
        :max="this.$props.maxPages"
        min="1"
        v-model.number="this.currentPage"
        @change="sendPageNum"
    >
  </div>
</template>

<style scoped>

</style>