<script>
import data from "./assets/data.json";
import TheTable from "./components/newtable.vue";
import TheCheckbox from "./components/checkboxes.vue";
import ThePageSelector from "./components/pageselector.vue";
import TheSearchBar from "./components/searchbar.vue"
import {ref} from "vue";

export default {
  components: {TheTable, TheCheckbox, ThePageSelector, TheSearchBar},
  created() {
    this.UIData = data;
    this.hidestatus = [];
    this.maxPages = Math.ceil(data.length / 100);
    this.page = 1;
    this.filterCategory = "";
    this.filterValue = "";
  },
  computed: {
    productDataBystatus() {
      let data = this.UIData;
      let tmp = {};
      let statusSet = new Set();

      let newData = [];
      // get rid of stuff that we want to hide by status
      data.forEach((element) => {
        // console.log(element);
        let status = element.Status;
        if (!this.hidestatus.includes(status)) { // Hide by status
          console.log(Object.keys(element));
          console.log(typeof(element.Base_Freq));
          // console.log(this.filterCategory);
          // console.log(this.filterValue);
          if (this.filterCategory === "" || element[this.filterCategory] === this.filterValue) {
            newData.push(element);
          }
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
    receiveFilterInfo(filterCat, filterVal) {
      this.filterCategory = filterCat;
      this.filterValue = filterVal;
      this.$refs.pageSelector.resetPageNum();
    }
  },
  data: () => {
    return {
      hidestatus: [],
      UIData: [],
      maxPages: ref(0),
      page: ref(1),
      filterCategory: ref(""),
      filterValue: ref(""),
    }
  },
  setup() {}
}
</script>

<template>
  <div id="app">
    <TheCheckbox :productDataByStatus="productDataBystatus" @hide-status="receiveHideStatus"/>
    <ThePageSelector :maxPages="maxPages" @new-page="receivePageNumber" ref="pageSelector"/>
    <TheSearchBar @new-search="receiveFilterInfo"/>
    <TheTable :productDataByStatus="productDataBystatus"/>
  </div>
</template>

<style>
  @import "./assets/font.css";
  html, body {
    font-family: "clear-font", sans-serif;
  }
</style>