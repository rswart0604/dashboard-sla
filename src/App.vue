<script>
import data from "./assets/data.json";
import TheTable from "./components/newtable.vue";
import TheCheckbox from "./components/checkboxes.vue";
import ThePageSelector from "./components/pageselector.vue";
import {ref} from "vue";

export default {
  components: {TheTable, TheCheckbox, ThePageSelector},
  created() {
    this.UIData = data;
    this.hidestatus = [];
    this.maxPages = Math.ceil(data.length / 100);
    this.page = 1;
  },
  computed: {
    productDataBystatus() {
      let data = this.UIData;
      let tmp = {};
      let statusSet = new Set();

      let newData = [];
      // get rid of stuff that we want to hide by status
      data.forEach((element) => {
        let status = element.Status;
        if (!this.hidestatus.includes(status)) { // Hide by status
          newData.push(element);
        }
      });

      // now that we've hidden stuff, change max page number
      this.maxPages = Math.ceil(newData.length / 100);

      // splice it according to our needs
      let slicedData = newData.slice((this.page-1)*100, (this.page)*100);


      // now go through that new data
      slicedData.forEach((element) => {
        let status = element.Status;
        let cores = element.Cores;

        if (!tmp[status]) tmp[status] = {};
        if (!tmp[status][cores]) tmp[status][cores] = [];

        tmp[status][cores].push(element);
      });

      // we still want to see all the statuses up at the top though
      data.forEach((element) => {
        let status = element.Status;
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
      this.$refs.pageSelector.resetPageNum();
    },
    receivePageNumber(newPageNum) {
      this.page = newPageNum;
    },
  },
  data: () => {
    return {
      hidestatus: [],
      UIData: [],
      maxPages: ref(0),
      page: ref(1),
    }
  },
  setup() {}
}
</script>

<template>
  <div id="app">
    <TheCheckbox :productDataByStatus="productDataBystatus" @hide-status="receiveHideStatus"/>
    <ThePageSelector :maxPages="maxPages" @new-page="receivePageNumber" ref="pageSelector"/>
    <TheTable :productDataByStatus="productDataBystatus"/>
  </div>
</template>