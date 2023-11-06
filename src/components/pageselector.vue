<script>
  export default {

    props: {
      maxPages: Number, // we'll get maxPages based on the data from dashboard.vue
    },

    data: () => {
      return {
        currentPage: 1, // initialize currentPage to 1
      }
    },

    methods: {
      sendPageNum() { // send page num back up to dashboard
        this.$emit('new-page', this.currentPage);
      },
      resetPageNum() {
        this.currentPage = 1;
        this.sendPageNum();
      },
      increasePageNum() { // see if we're at the max before increasing page num
        if (this.currentPage < this.$props.maxPages) {
          this.currentPage++;
          this.sendPageNum();
        }
      },
      decreasePageNum() { // see if we're at 1 before decreasing page num
        if (this.currentPage > 1) {
          this.currentPage--;
          this.sendPageNum();
        }
      }
    }

  }
</script>

<template>
  <div class="selector">
    <input
        class="left-button"
        type="button"
        @click="decreasePageNum"
        value="<"
        :disabled="this.currentPage <= 1"
    >
    <input
        type="number"
        :max="this.$props.maxPages"
        min="1"
        v-model.number="this.currentPage"
        @change="sendPageNum"
    >
    <input
        class="right-button"
        type="button"
        @click="increasePageNum"
        value=">"
        :disabled="this.currentPage >= this.$props.maxPages"
    >
  </div>
</template>

<style scoped>

</style>