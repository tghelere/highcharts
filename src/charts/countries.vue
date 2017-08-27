<script>
import {mapState} from 'vuex'
import _ from 'lodash'
import Highcharts from 'highcharts'

export default {
    computed: mapState({
        list: state => state.list
    }),

    watch: {
        list(){
            this.dataSource()
        }
    },

    methods: {
        // Used to read the list and calculate the chart data source
        dataSource(){
            const countries = this.list.map(item => item.country)

            const base = _(countries)
                .countBy()
                .map((value, key) => ({key, value}))
                .orderBy(['value'], ['desc'])
                .value()

            const categories = base.map(item => item.key)
            
            const values = base.map(item => item.value)            
            
            this.setup({categories, values})
        },

        // Used to mount the chart and display it on the screen
        // Note: this method should only be called when the data source is ready
        setup(obj){
            
            const {categories, values} = obj

             Highcharts.chart('container-for-charts', {
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
    <div id="container-for-charts">
        Countries chart
    </div>
</template>