<script>
import base from './base.vue'
import _ from 'lodash'
import Highcharts from 'highcharts'

export default {
    extends: base,
    methods: {
        // Used to read the list and calculate the chart data source
        dataSource(){
            const countries = this.localList.map(item => item.country)

            const base = _(countries)
                .countBy()
                .map((value, key) => ({key, value}))
                .orderBy(['value'], ['desc'])
                .value()

            const categories = base.map(item => item.key)
            
            const values = base.map(item => item.value)  
            
            if (this.chart === null) {
                this.chart = this.setup({ categories, values })
            }else{
                this.chart.series[0].setData(values)
            }
                        
        },

        // Used to mount the chart and display it on the screen
        // Note: this method should only be called when the data source is ready
        setup(obj){
            
            const {categories, values} = obj

             return Highcharts.chart('container-for-countries', {
                chart: {
                    type: 'column'
                },
                title: {
                    text: 'Countries'
                },
                subtitle: {
                    text: 'Source: github.com/tghelere'
                },
                xAxis: {
                    categories: categories,
                    crosshair: true
                },
                yAxis: {
                    min: 0,
                    title: {
                        text: 'Countries'
                    }
                },            
                series: [{
                    name: 'Quantity',
                    data: values

                }]
            });
        },
    },
}
</script>

<template>
    <div id="container-for-countries"></div>
</template>