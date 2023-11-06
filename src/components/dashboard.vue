<script>
import data from "../assets/data.json";
import Table from "./table.vue";
import Checkboxes from "./checkboxes.vue";
import PageSelector from "./pageselector.vue";
import SearchBar from "./searchbar.vue"
import {ref} from "vue";

export default {
  components: {Table, Checkboxes, PageSelector, SearchBar},
  created() { // initialize data
    this.UIData = data;
    this.hidestatus = [];
    this.maxPages = Math.ceil(data.length / 100); // calculate an initial maxPages so we have 100 per page
    this.page = 1;
    this.filterCategory = "";
    this.filterValue = "";
  },
  computed: {
    // go through our whole data set and select what we want
    // then, return it in a presentable fashion
    productDataBystatus() {
      let data = this.UIData;
      let tmp = {};
      let statusSet = new Set();

      let newData = [];
      // get rid of stuff that we want to hide by status
      data.forEach((element) => {
        let status = element.Status;
        // Hide by status
        if (!this.hidestatus.includes(status)) {
          // Select based on filter (or let it through if we have no filter)
          if (this.filterCategory === "" || element[this.filterCategory] === this.filterValue) {
            newData.push(element);
          }
        }
      });

      // now that we've hidden stuff, change max page number accordingly
      this.maxPages = Math.ceil(newData.length / 100);

      // splice it according to our needs
      let slicedData = newData.slice((this.page-1)*100, (this.page)*100);

      // now go through that new data to format it correctly
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
    receiveHideStatus(newHideStatusList) { // get the list from the checkboxes of stuff we want to hide
      this.hidestatus = newHideStatusList;
      this.$refs.pageSelector.resetPageNum();
    },
    receivePageNumber(newPageNum) { // get the page num from PageSelector
      this.page = newPageNum;
    },
    receiveFilterInfo(filterCat, filterVal) { // get the filter info from the search bar
      this.filterCategory = filterCat;
      this.filterValue = filterVal;
      this.$refs.pageSelector.resetPageNum();
    }
  },
  data: () => {
    return { // all the data we use
      hidestatus: [], // all the statuses we want to hide
      UIData: [], // data for prseenting
      maxPages: ref(0), // the max number of pages for the PageSelector
      page: ref(1), // the current page (from PageSelector)
      filterCategory: ref(""), // the attribute we want to filter on
      filterValue: ref(""), // the value we want the filter attribute to equal
    }
  },
  setup() {}
}
</script>

<template>
  <div class="whole" id="app">
    <div class="header">
      <Checkboxes class="checkboxComponent" :productDataByStatus="productDataBystatus" @hide-status="receiveHideStatus"/>
      <div class="secondRow">
        <PageSelector :maxPages="maxPages" @new-page="receivePageNumber" ref="pageSelector"/>
        <SearchBar @new-search="receiveFilterInfo"/>
      </div>
    </div>
    <Table :productDataByStatus="productDataBystatus"/>
  </div>
</template>

<style scoped>
@import "../assets/font.css";
html, body {
  font-family: "clear-font", sans-serif;
}

.secondRow {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;

}

.checkboxComponent {
  margin-bottom: 5px;
}

.header {
  background-color: #9ef5ff;
  border: 10px solid #9ef5ff;
}
</style>
