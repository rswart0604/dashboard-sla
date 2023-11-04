<script>
  import {ref} from "vue";

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
      },
      resetPageNum() {
        this.currentPage = 1;
        this.sendPageNum();
      },
      increasePageNum() {
        if (this.currentPage < this.$props.maxPages) {
          this.currentPage++;
          this.sendPageNum();
        }
      },
      decreasePageNum() {
        if (this.currentPage > 1) {
          this.currentPage--;
          this.sendPageNum();
        }
      }
    }

  }
</script>

<template>
  <div>
    <input
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
        type="button"
        @click="increasePageNum"
        value=">"
        :disabled="this.currentPage >= this.$props.maxPages"
    >
  </div>
</template>

<style scoped>

</style>