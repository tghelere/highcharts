<script>
    export default {
        data(){
            return{
                gender: ''
            }
        },
        methods: {
            commit(){
                this.$store.commit('SET_FILTER', this.gender)
            }
        }
    }
</script>
<template>
    <div>
        <select @change="commit" v-model="gender">
            <option value="">Both</option>
            <option value="Male">Male</option>
            <option value="Female">Female</option>
        </select>
    </div>
</template>