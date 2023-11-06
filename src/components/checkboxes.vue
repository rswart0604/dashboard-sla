<script>
export default {
  props: {
    productDataByStatus: Object // get product list from dashboard.bue
  },

  data: function() {
    return {
      allCheckBox: [],
      hideStatusList: [],
    };
  },

  methods: {
    // we clicked "hide all" so go through and check it
    hideShowALLstatus() {
      if (!document.querySelector(".styled").checked) {
        this.hideStatusList = [];
        this.allCheckBox = [];
      }

      if (document.querySelector(".styled").checked) {
        this.hideStatusList = this.productDataByStatus.status;
        this.allCheckBox = this.productDataByStatus.status;
      }

      this.allCheck = !this.allCheck;

      if (!this.allCheck) {
        this.hideStatusList = [];
        this.allCheckBox = [];
      }
      this.tableHideStatus();
    },

    // send our hideStatusList to table.vue
    tableHideStatus() {
      this.$emit('hide-status', this.hideStatusList);
    }
  },

  setup(props) {
  }
}
</script>

<template>
  <!-- Hide By status Bar -->
  <div class="hideBar">
    <label class="hideLabel"> Hide: </label>
    <div class="checkbox">
      <!-- All status -->
      <div class="box">
        <input
            :id="this.$props.productDataByStatus.status"
            type="checkbox"
            class="styled"
            :value="this.$props.productDataByStatus.status"
            @click="hideShowALLstatus"
            v-model="hideStatusList"
        />
        <label :for="this.$props.productDataByStatus.status">All statuses</label>
      </div>


      <!-- Dynamic status -->
      <div class="box" v-for="status in this.$props.productDataByStatus.status" :key="`${status}`">
        <input
            :id="`${status}`"
            type="checkbox"
            class="styled"
            :value="status"
            @change="tableHideStatus"
            v-model="hideStatusList"
        />
        <label :for="`${status}`">
          {{ status }}
        </label>
      </div>
    </div>
  </div>
</template>

<style scoped>
.checkbox {
  display: flex;
}

.checkbox label {
  margin-left: 0;
  margin-right: 10px;
}

.box:hover {
  background-color: #ccc;
}

.hideBar {
  background-color: #9ef5ff;
}

</style>