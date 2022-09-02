<template>
    <div class="chart-size">
        <canvas id="myChart" width="600" height="400"></canvas>
    </div>
</template>


<script>
import Chart from 'chart.js/auto';

export default {
    name: 'Barchart',
    props: {
        data: Array,
    },
    watch: {
        data: function (newVal, oldVal) { // watch it
            this.myChart.data.datasets[0].data = newVal
            this.myChart.update()
        },
    },
    data() {
        return {
            myChart: ''
        }
    },
    methods: {
        updateBarchart() {

        },
    },
    mounted() {
        const ctx = document.getElementById('myChart');
        // myChart;
        this.myChart = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: ['Temp', 'EC2', 'HumID', 'CO2'],
                datasets: [{
                    label: 'temp',
                    data: this.data,
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.6)',
                        'rgba(54, 162, 235, 0.6)',
                        'rgba(75, 192, 192, 0.6)',
                        'rgba(153, 102, 255, 0.6)',
                    ],
                    borderColor: [
                        'rgba(255, 99, 132, 1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });

    }
}
</script>


<style>
.chart-size {
    width: 400;
    margin-left: -12rem;
    padding-left: 180px;
}
</style>