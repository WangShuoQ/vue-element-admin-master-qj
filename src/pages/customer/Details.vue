<template>
    <div class="customerDetails">
<el-button size="small" type="text" @click="backHandler">返回</el-button>
    <el-tabs v-model="activeName">
        <el-tab-pane label="基本信息" name="info">基本信息...</el-tab-pane>
        <el-tab-pane label="订单信息" name="orders">订单信息...</el-tab-pane>
        <el-tab-pane label="服务地址" name="address">服务地址...</el-tab-pane>
    </el-tabs>
    </div>
</template>
<script>
import {mapState,mapActions} from 'vuex'
export default {
    data(){
        return {
            activeName:"info"
        }
    },
    created(){

    },
    computed:{

    },
    methods:{
        backHandler(){
            //this.$router.push("/customer")
            this.$router.go(-1)
        }
    }
}
</script>
<style scoped>

</style>