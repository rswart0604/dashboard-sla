<script>
import data from "./assets/data.json";
import TheTable from "./components/newtable.vue";
import TheCheckbox from "./components/checkboxes.vue";
import {ref} from "vue";

export default {
  components: {TheTable, TheCheckbox},
  created() {
    this.UIData = data;
    this.hidestatus = [];
    this.maxPages = Math.floor(data.length / 100) + 1;
  },
  computed: {
    productDataBystatus() {
      let data = this.UIData;
      let tmp = {};
      let statusSet = new Set();

      // get it to be less than or equal to 100 at whatever page index the user wants
      let slicedData = data.slice(this.page*100, (this.page+1)*100);

      slicedData.forEach((element) => {
        let status = element.Status;
        let cores = element.Cores;

        if (this.hidestatus.includes(status)) return; // Hide by status
        if (!tmp[status]) tmp[status] = {};
        if (!tmp[status][cores]) tmp[status][cores] = [];

        tmp[status][cores].push(element);
      });

      // we still want to see all the statuses up at the top
      data.forEach((element) => {
        let status = element.Status;

        // push status to set
        statusSet.add(status);
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
  methods: {
    receiveHideStatus(newHideStatusList) {
      this.hidestatus = newHideStatusList;
    }
  },
  data: () => {
    return {
      hidestatus: [],
      UIData: [],
      maxPages: 0,
      page: ref(3),
    }
  },
  setup() {}
}

</script>

<template>
  <div id="app">
    <TheCheckbox :productDataByStatus="productDataBystatus" @hide-status="receiveHideStatus"/>
    <TheTable :productDataByStatus="productDataBystatus"/>
  </div>
</template>