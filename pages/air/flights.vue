<template>
    <section class="contianer">
        <el-row  type="flex" justify="space-between">

            <!-- 顶部过滤列表 -->
            <div class="flights-content">
                <!-- 过滤条件 -->
                <div>

                </div>

                <!-- 航班头部布局 -->
                 <FlightsItem 
                    v-for="(item, index) in dataList"
                    :key="index"
                    :data="item"/>

                     <!-- 分页 -->
                    <!-- size-change：每页条数切换时候触发 -->
                    <!-- current-change：页码切换时候触发 -->
                    <!-- current-page: 当前的页码 -->
                    <!-- page-size: 当前显示的条数 -->
                    <!-- total: 总条数 -->
                    <el-pagination
                        @size-change="handleSizeChange"
                        @current-change="handleCurrentChange"
                        :current-page="pageIndex"
                        :page-sizes="[5, 10, 15, 20]"
                        :page-size="pageSize"
                        layout="total, sizes, prev, pager, next, jumper"
                        :total="total">
                    </el-pagination>


                <!-- 航班信息 -->
                <div>
                <FlightsItem></FlightsItem>
                </div>
            </div>

            <!-- 侧边栏 -->
            <div class="aside">
                <!-- 侧边栏组件 -->
            </div>
        </el-row>
    </section>
</template>

<script>
import FlightsListHead from "@/components/air/flightsListHead.vue"
import FlightsItem from "@/components/air/flightsItem.vue";

export default {
    data(){
        return {
            // 机票列表返回的总数据，总数据包含4个属性，flights/info/options/tatol
            flightsData: {},
            // 当前显示的列表数组
            dataList: [],
            pageIndex:1, // 当前的页码
            pageSize: 5, // 当前的条数
            total: 0     // 总条数
        }
    },
    components: {
        FlightsListHead,
        FlightsItem
    },
    methods: {
        // 每页条数切换时候触发
        // 每页条数切换时候触发, val是条数
        handleSizeChange(val){
            this.pageSize = val;
            // 按照数学公式切换dataList的值
            this.dataList = this.flightsData.flights.slice(0, val);
        },
        // 页码切换时候触发
         handleCurrentChange(val){
               this.pageIndex = val; // 当前页
            // 按照数学公式切换dataList的值
                this.dataList = this.flightsData.flights.slice( 
                (this.pageIndex - 1) * this.pageSize, 
                this.pageIndex * this.pageSize 
            );
            }
    },
    mounted(){
        // 请求航班列表数据
        this.$axios({
            url: "airs",
            // 路由的url参数
            params: this.$route.query
        }).then(res => {
            // 赋值给总数据
            this.flightsData = res.data;
            // 分页的总条数
            this.total =  this.flightsData.flights.length;
            // 第一页的值
            this.dataList = this.flightsData.flights.slice(0, this.pageSize);

        })
    }
}
</script>

<style scoped lang="less">
    .contianer{
        width:1000px;
        margin:20px auto;
    }
    .flights-content{
        width:745px;
        font-size:14px;
    }
    .aside{
        width:240px;
    } 
</style>