<script>
import data from "./assets/data.json";
import TheTable from "./components/newtable.vue";
import TheCheckbox from "./components/checkboxes.vue";

export default {
  components: {TheTable, TheCheckbox},
  mounted() {
    this.UIData = data;
  },
  computed: {
    productDataBystatus() {
      let data = this.UIData;
      let tmp = {};
      let statusSet = new Set();

      data.forEach((element) => {
        let status = element.Status;
        let cores = element.Cores;

        // push status to set
        statusSet.add(status);

        if (this.hidestatus.includes(status)) return; // Hide by status
        if (!tmp[status]) tmp[status] = {};
        if (!tmp[status][cores]) tmp[status][cores] = [];

        tmp[status][cores].push(element);
      });

      // sort status in order
      const strings = new Set(statusSet);
      const sortedStringsArray = [...strings].sort();
      statusSet = new Set(sortedStringsArray);

      return {
        status: [...statusSet],
        data: tmp,
      };
    },
  },
  data: () => {
    return {
      hidestatus: [],
      UIData: [],
    }
  },
  setup() {}
}

</script>

<template>
  <div id="app">
    <TheCheckbox :productDataByStatus="productDataBystatus"/>
    <TheTable :productDataByStatus="productDataBystatus"/>
  </div>
</template>