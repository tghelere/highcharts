<script>
import base from './base.vue'
import _ from 'lodash'
import Highcharts from 'highcharts'
export default {
    extends: base,

    methods: {
        dataSource(){
            const languages = this.localList.map(item => item.language)

            const base = _(languages)
                .countBy()
                .map((y, name) => ({ y, name }))
                .orderBy(['value'], ['desc'])
                .value()

            const finalData = base.slice(0, 10)

            if (this.chart === null) {
                this.chart = this.setup({ finalData })                
            }else{
                this.chart.series[0].setData(finalData)
            }
            
        },
        setup(obj){
            const { finalData } = obj
            return Highcharts.chart('container-for-languages', {
                chart: {
                    plotBackgroundColor: null,
                    plotBorderWidth: null,
                    plotShadow: false,
                    type: 'pie'
                },
                title: {
                    text: 'Languages'
                },
                tooltip: {
                    pointFormat: '{series.name}: <b>{point.percentage:.1f}%</b>'
                },
                plotOptions: {
                    pie: {
                        allowPointSelect: true,
                        cursor: 'pointer',
                        dataLabels: {
                            enabled: true,
                            format: '<b>{point.name}</b>: {point.percentage:.1f} %',
                            style: {
                                color: (Highcharts.theme && Highcharts.theme.contrastTextColor) || 'black'
                            }
                        }
                    }
                },                
                series: [{
                    name: 'Percentage',
                    colorByPoint: true,
                    data: finalData
                }]
            });
        }
    }
}
</script>

<template>
    <div id="container-for-languages"></div>
</template>