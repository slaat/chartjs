<template>
  <canvas class="chartjs" :width="width" :height="height"></canvas>
</template>

<script>
import Chart from 'chart.js' // With moment.js

const types = ['line', 'bar', 'radar', 'polarArea', 'pie', 'doughnut']

export default {

  props: {
    width: Number,
    height: Number,
    type: {
      type: String,
      required: true,
      validator (value) {
        return types.indexOf(value) > -1
      }
    },
    data: {
      type: Object,
      required: true,
      default: () => ({})
    },
    options: {
      type: Object,
      default: () => ({})
    }
  },

  mounted () {
    this.chart = new Chart(this.$el, {
      type: this.type,
      data: this.data,
      options: this.options
    })
  },

  data () { return {
    chart: null
  }},

  methods: {
    resetChart () {
      this.$nextTick(() => {
        this.chart.destroy()
        this.chart = new Chart(this.$el, {
          type: this.type,
          data: this.data,
          options: this.options
        })
      })
    }
  },

  watch: {
    type () {
      this.resetChart()
    },
    data () {
      this.resetChart()
    },
    options () {
      this.resetChart()
    }
  }
}
</script>

<style lang="scss">
canvas.chartjs {
  max-width: 100%;
}
</style>
