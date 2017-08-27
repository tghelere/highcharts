<script>
import {mapState} from 'vuex'
import _ from 'lodash'

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
            
        },
    },
}
</script>

<template>
    <div>
        Countries chart
    </div>
</template>