<script>
  export default {
    props: { // get the product data list
      productDataByStatus: Object
    },

    data: () => {
      return {
        wwInfo: {},
        colors: ["rgb(169, 212, 199)", "rgb(237, 226, 214)", "rgb(182, 197, 219)", "rgb(250, 205, 205)",
                 "rgb(224, 187, 228)", "rgb(255, 223, 211)", "rgb(181, 207, 209)", "rgb(217, 228, 230)"],
        statusToColors: {}
      }
    },

    mounted() {
      this.wwInfo = this.getWWFromDate();
    },

    computed: {
      wwData() { // do work week stuff
        return `${this.wwInfo.year}WW${this.wwInfo.workweek}.${this.wwInfo.numofday}`;
      },
    },

    methods: {
      getWWFromDate(date = null) { // do more workweek stuff
        let currentDate = date || new Date();
        let startDate = new Date(currentDate.getFullYear(), 0, 1);
        let days = Math.floor((currentDate - startDate) / (24 * 60 * 60 * 1000));

        return {
          year: currentDate.getFullYear(),
          workweek: Math.ceil(days / 7),
          numofday: currentDate.getDay(),
        };
      },

      calstatusRowspan(data) { // calculate how large a row should be
        let sum = Object.keys(data).length + 1;
        for (const cores in data) {
          sum += Object.keys(data[cores]).length;
        }
        return sum;
      },

      getRowStyle(status) { // get the color for the row based on the status
        if (!(status in this.statusToColors)) {
          this.statusToColors[status] = this.colors.pop();
        }
        return "background: " + this.statusToColors[status];
      }
    },
    setup(props) {}
  }
</script>

<template>
  <div>
    <!-- Main Table Design -->
    <table>
      <thead>
      <tr>
        <td :colspan="12">Dashboard SLA</td>
      </tr>
      <tr>
        <th colspan="3">{{ wwData }}</th>
        <th colspan="8">Product Info</th>
      </tr>
      <tr>
        <th>Status</th>
        <th>Cores</th>
        <th class="width1">Product</th>
        <th class="width1">Lithography</th>
        <th>Threads</th>
        <th>Base Freq</th>
        <th>Max Turbo Freq</th>
      </tr>
      </thead>
      <tbody>
      <template v-for="(data, status) in this.$props.productDataByStatus.data">
        <!-- status -->
        <tr :style="getRowStyle(status)">
          <td class="width1" :rowspan="calstatusRowspan(data)" >
            {{ status }}
          </td>
        </tr>

        <template v-for="cores in Object.keys(data)">
          <!-- cores -->
          <tr :style="getRowStyle(status)">
            <td class="width1" :rowspan="Object.keys(data[cores]).length + 1">
              {{ cores }}
            </td>
          </tr>

          <tr v-for="v in data[cores]" :style="getRowStyle(status)">
            <!-- product -->
            <td class="productColumn" :style="getRowStyle(status)">{{ v.Product }}</td>

            <!-- Lithography -->
            <td>{{ v.Lithography }}</td>

            <!-- Threads -->
            <td>
              <div class="innerCells">
                <input :value="v.Threads" :disabled="true" type="text" />
              </div>
            </td>

            <!-- Base Freq -->
            <td>
              <div class="innerCells">
                <input :value="v.Base_Freq" :disabled="true" type="text" />
              </div>
            </td>

            <!-- Max Turbo Freq -->
            <td>
              <div class="innerCells">
                <input :value="v.Max_Turbo_Freq" type="text" :disabled="true" />
              </div>
            </td>
          </tr>
        </template>
      </template>
      </tbody>
    </table>
    <!-- End of Table Design -->
  </div>
</template>

<style scoped>

.fas.fa-times {
  display: none;
}

.fas.fa-times.comment {
  display: block;
}

.overWrittenCells:hover .fas {
  display: block;
}

.innerCells {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.innerCells.comment {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 15px;
}

table {
  width: 100%;
  white-space: nowrap !important;
}

table td {
  position: relative;
}

i {
  cursor: pointer;
}

.legendColorBox {
  margin: 0.4%;
  float: left;
  height: 20px;
  width: 30px;
  border: 1px solid grey;
  margin-right: 4%;
}


.overWrittenCells {
  border: 2px solid rgb(194, 1, 1);
}

.overWrittenCells input {
  outline: 0;
}

input::placeholder {
  color: black;
}

input:focus::-webkit-input-placeholder {
  color: grey;
}

input[disabled] {
  cursor: text;
  background-color: inherit;
  color: black;
}

.legend-labels li {
  font-size: small;
  margin-right: 2%;
}

select {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  text-align: center;
  border: 0;
}

table tr td:not(.skip),
table tr th {
  text-align: center;
}

td,
th {
  padding: 2px !important;
  width: 100px;
  border: 1px solid black;
}

.productColumn {
  width: 1%;
  background-color: white;
}


.checkbox label {
  margin-left: 10px;
}

.width1 {
  width: 1%;
  /* white-space: nowrap !important; */
}
</style>