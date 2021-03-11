<template>
  <div class="hello">
    <GChart
    class="chart"
    type="PieChart"
    :data="chartData"
    :options="chartOptions"
  />
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      // Array will be automatically processed with visualization.arrayToDataTable function
      chartData: [
        ['Task', 'Hours per Day'],
        ['Work',     0],
          ['Eat',      2],
          ['Commute',  2],
          ['Watch TV', 2],
          ['Sleep',    7]
      ],
      chartOptions: {
        chart: {
          title: 'Company Performance',
          subtitle: 'Sales, Expenses, and Profit: 2014-2017',
          
        },
        pieHole: 0.4,
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .chart{
    width: 1000px;
    min-height: 500px;
  }

</style>
