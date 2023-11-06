<script>
  export default {
    props: {},
    data: () => {
      return {
        search_value: ""
      }
    },
    emits: ["new-search"],
    methods: {
      empty() {
        if (this.search_value === "") {
          this.searchEntered();
        }
      },
      searchEntered() {
        // some formatting for search value
        const searchString = this.search_value;
        // search on status,
        if (searchString.startsWith("Status:")) {
          this.$emit("new-search", "Status", searchString.substring(7).trim());
        }
        else if (searchString.startsWith("Cores:")) {
          this.$emit("new-search", "Cores", Number(searchString.substring(6).trim()));
        }
        else if (searchString.startsWith("Product:")) {
          this.$emit("new-search", "Product", searchString.substring(8).trim());
        }
        else if (searchString.startsWith("Lithography:")) {
          this.$emit("new-search", "Lithography", Number(searchString.substring(12).trim()));
        }
        else if (searchString.startsWith("Threads:")) {
          this.$emit("new-search", "Threads", Number(searchString.substring(8).trim()));
        }
        else if (searchString.startsWith("Base Freq:")) {
          this.$emit("new-search", "Base_Freq", Number(searchString.substring(10).trim()));
        }
        else if (searchString.startsWith("Max Turbo Freq:")) {
          this.$emit("new-search", "Max_Turbo_Freq", Number(searchString.substring(15).trim()));
        } else {
          this.$emit("new-search", "", "");
        }
      }
    },
  }
</script>

<template>
  <div>
    <input
        v-model="search_value"
        placeholder="Search"
        @input="empty"
    >
    <input
        class="button"
        type="button"
        @click="searchEntered"
        value="Enter"
    >
  </div>
</template>

<style scoped>
.button {
  background-color: #b5cfd1;
}
</style>
