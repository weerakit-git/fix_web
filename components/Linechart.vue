<template>
  <div id="content">
    <div id="name_topic" class="col-topic">
      <h1>ข้อมูลภาพรวมของพืช</h1>
    </div>
      
    <div id="Line_chart" class="line-ch">
      <br><br><br>
      <canvas id="myChart1" width="300" height="100"></canvas>
    </div>
    <hr>
  </div>
  
</template>
  
  
  <script>
import Chart from 'chart.js/auto';

export default {
  props: {
    data: Array,
  },
  watch: {
    data: function (newVal, oldVal) { // watch it
      this.createGraph(newVal)
    },
  },
  methods: {
    createGraph(data) {
      const ctx = document.getElementById('myChart1');

      var temp = this.data.map(function (elem) {
        return elem.temp;
      });
      var humid = this.data.map(function (elem) {
        return elem.humid;
      });
      var co2 = this.data.map(function (elem) {
        return elem.co2;
      });
      var ec = this.data.map(function (elem) {
        return elem.ec;
      });
      var created_at = this.data.map(function (elem) {
        return elem.created_at;
      });

      const myChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: created_at,
          datasets: [
            {
              label: 'tepm',
              data: temp,
              backgroundColor: 'rgba(33, 95, 238, 0.6)',
              borderColor: 'rgb(33, 95, 238)',
              borderWidth: 3
            },
            {
              label: 'hum',
              data: humid,
              backgroundColor: 'rgba(255, 99, 132, 0.6)',
              borderColor: 'rgba(255, 99, 132, 1)',
              borderWidth: 3
            },
            {
              label: 'co2',
              data: co2,
              backgroundColor: 'rgba(153, 102, 255, 0.6)',
              borderColor: 'rgb(153, 102, 255)',
              borderWidth: 3
            },
            {
              label: 'ec',
              data: ec,
              backgroundColor: 'rgba(126, 185, 41, 0.6)',
              borderColor: 'rgb(126,185,41)',
              borderWidth: 3
            },
          ]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
      myChart;
    }
  },
  mounted() { }
}

</script>
  
  
  <style>
  
h1 {
    text-align: center;
    font-size: 50px;
    border-style: solid;
    margin-top: -1rem;
    border-radius: 50px;
    
  }
  #content{
    padding: 50px;
  }

  </style>
  
  