<template>
  <apexchart type="line" :options="options" :series="series" :stroke="stroke"></apexchart>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Chart',
  props: {
    data: {
      type: Array
    }
  },
  data: function () {
    return {
      options: {
        chart: {
          id: 'vuechart-example'
        },
        stroke: {
          curve: 'smooth',
          width: 2
        },
        xaxis: {
          type: 'datetime'
        }
      },
      series: [{
        name: 'counter',
        data: this.data
      }]
    }
  }

})
</script>

<style>

</style>
