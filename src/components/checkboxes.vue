<script>
export default {
  props: {
    productDataByStatus: Object
  },

  data: function() {
    return {
      allCheckBox: [],
      hideStatusList: [],
    };
  },

  methods: {
    // we clicked "hide all" so pls do that
    hideShowALLstatus() {
      if (!document.querySelector(".styled").checked) {
        this.hideStatusList = [];
        this.allCheckBox = [];
      } else {
        this.hideStatusList = this.productDataBystatus.status;
        this.allCheckBox = this.productDataBystatus.status;
      }

      this.allCheck = !this.allCheck;

      if (!this.allCheck) {
        this.hideStatusList = [];
        this.allCheckBox = [];
      }
    },

    // send our hideStatusList to table.vue
    hideStatusHandler() {
      this.$emit('hideStatus', this.hideStatusList);
    }
  },
  setup(props) {
    console.log(props)
  }
}
</script>

<template>
  <!-- Hide By status Bar -->
  <div class="hideBar">
    <label class="hideLabel"> Hide: </label>
    <div class="checkbox">
      <!-- All status -->
      <input
          :id="this.$props.productDataByStatus.status"
          type="checkbox"
          class="styled"
          :value="this.$props.productDataByStatus.status"
          @click="hideShowALLstatus"
          v-model="hideStatusList"
      />
      <label :for="this.$props.productDataByStatus.status">All statuses</label>

      <!-- Dynamic status -->
      <div v-for="status in this.$props.productDataByStatus.status" :key="`${status}`">
        <input
            :id="`${status}`"
            type="checkbox"
            class="styled"
            :value="status"
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

</style>