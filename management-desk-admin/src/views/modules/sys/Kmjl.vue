<template>
    <div class="mod-config">
        <el-form ref="form" :model="form" :inline="true">
            <el-form-item label="锁id">
                <el-input placeholder="锁id" v-model="form.lockId"></el-input>
            </el-form-item>
            <el-form-item label="用户id">
                <el-input placeholder="用户id" v-model="form.userId"></el-input>
            </el-form-item>
            <el-form-item label="开锁方式">
                <el-select v-model="unlockingWayValue" @change="choseUnlockingWay" placeholder="请选择">
                    <el-option
                    v-for="item in unlockingWay"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                    </el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="开锁结果">
                <el-select v-model="unlockingResultValue" @change="choseunlockingResult" placeholder="请选择">
                    <el-option
                    v-for="item in unlockingResult"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                    </el-option>
                </el-select>
            </el-form-item>
            <el-form-item>
                <el-button>查询</el-button>
            </el-form-item>
            <el-form-item label="定时刷新" label-width="90px">
                <el-input placeholder="定时刷新" type="number"></el-input>
            </el-form-item>
        </el-form>
        <el-table :data="tableData" style="width: 100%;" border>
            <el-table-column
                align="center" 
                header-align="center" 
                prop="lockId"
                label="锁id">
            </el-table-column>
            <el-table-column
                align="center" 
                header-align="center" 
                prop="userId"
                label="用户id">
            </el-table-column>
            <el-table-column
                align="center" 
                header-align="center" 
                prop="uploadWay"
                label="上传方式">
            </el-table-column>
            <el-table-column
                align="center" 
                header-align="center" 
                prop="unlockingWay"
                label="开锁方式">
            </el-table-column>
            <el-table-column
                align="center" 
                header-align="center" 
                prop='unlockingTime'
                label="开锁时间">
            </el-table-column>
            <el-table-column
                align="center" 
                header-align="center" 
                prop="unlockingResult"
                label="开锁结果">
            </el-table-column>
            <el-table-column
                align="center" 
                header-align="center" 
                prop="uploadTime"
                label="上传时间">
            </el-table-column>
        </el-table>
        <el-pagination
            @current-change="handleCurrentChange"
            :current-page="currentPage"
            :page-size="pageSize"
            :page-sizes="[10, 20, 30, 40]"
            layout="total, sizes, prev, pager, next, jumper"
            :total="21">
        </el-pagination>
    </div>
</template>
<script>
    export default {
        data(){
            return{
                 form: {
                    lockId:"",
                    userId:""
                },
                unlockingWay:[{
                        value: 0,
                        label: '全部'
                    },
                    {
                        value:1,
                        label:'app'
                    },
                    {
                        value: 2,
                        label: '蓝扣'
                }],
                unlockingWayValue: '',
                unlockingResult:[{
                        value: 0,
                        label: '全部'
                    },
                    {
                        value:1,
                        label:'成功'
                    },
                    {
                        value: 2,
                        label: '失败'
                }],
                unlockingResultValue:'',
                tableData: [
                //     {
                //     id:1,
                //     lockId: 'AILOCK_4143bbf2f8f0',
                //     userId:"18968060066",
                //     uploadWay:"APP",
                //     unlockingWay:"app",
                //     unlockingTime: '2018-07-22 22:37:07',
                //     unlockingResult:"成功",
                //     uploadTime:"2018-07-23 21:14:50",
                // },
                // {
                //     id:2,
                //     lockId: 'AILOCK_dd2abbf2f8f0',
                //     userId:"13989529272",
                //     uploadWay:"APP",
                //     unlockingWay:"蓝扣",
                //     unlockingTime: '2018-07-16 17:00:51',
                //     unlockingResult:"成功",
                //     uploadTime:"2018-07-16 17:00:51",
                // },
                // {
                //     id:3,
                //     lockId: 'AILOCK_5438bbf2f8f0',
                //     userId:"13429153308",
                //     uploadWay:"APP",
                //     unlockingWay:"app",
                //     unlockingTime: '2018-05-25 19:07:26',
                //     unlockingResult:"成功",
                //     uploadTime:"2018-05-25 19:07:26",
                // }, 
                // {
                //     id:4,
                //     lockId: 'AILOCK_5438bbf2f8f0',
                //     userId:"13429153308",
                //     uploadWay:"APP",
                //     unlockingWay:"app",
                //     unlockingTime: '2018-05-25 17:28:47',
                //     unlockingResult:"失败",
                //     uploadTime:"2018-05-25 17:28:47",
                // },
                // {
                //     id:5,
                //     lockId: 'AILOCK_5438bbf2f8f0',
                //     userId:"13858117971",
                //     uploadWay:"APP",
                //     unlockingWay:"蓝扣",
                //     unlockingTime: '2018-05-09 16:55:19',
                //     unlockingResult:"失败",
                //     uploadTime:"2018-05-09 16:55:19",
                // },
                // {
                //     id:6,
                //     lockId: 'AILOCK_5438bbf2f8f0',
                //     userId:"13634132011",
                //     uploadWay:"APP",
                //     unlockingWay:"app",
                //     unlockingTime: '2018-05-09 16:49:23',
                //     unlockingResult:"成功",
                //     uploadTime:"2018-05-09 16:49:23",
                // },
                // {
                //     id:7,
                //     lockId: 'AILOCK_dd2abbf2f9f0',
                //     userId:"13858117971",
                //     uploadWay:"APP",
                //     unlockingWay:"app",
                //     unlockingTime: '2018-05-08 20:21:28',
                //     unlockingResult:"成功",
                //     uploadTime:"2018-05-08 20:21:30",
                // },
                // {
                //     id:8,
                //     lockId: 'AILOCK_4143bbf2f8f0',
                //     userId:"13989529272",
                //     uploadWay:"APP",
                //     unlockingWay:"蓝扣",
                //     unlockingTime: '2018-07-22 21:28:17',
                //     unlockingResult:"成功",
                //     uploadTime:"2018-07-22 21:28:17",
                // },
                // {
                //     id:9,
                //     lockId: 'AILOCK_4143bbf2f8f0',
                //     userId:"13666655668",
                //     uploadWay:"APP",
                //     unlockingWay:"app",
                //     unlockingTime: '2018-07-22 22:28:17',
                //     unlockingResult:"成功",
                //     uploadTime:"2018-07-22 22:28:17",
                // },
                // {
                //     id:10,
                //     lockId: 'AILOCK_4143bbf2f8f0',
                //     userId:"13566655668",
                //     uploadWay:"APP",
                //     unlockingWay:"app",
                //     unlockingTime: '2018-07-23 22:28:17',
                //     unlockingResult:"成功",
                //     uploadTime:"2018-07-22 23:28:17",
                // }
                ],
                totalCount:0,
                currentPage:1,
                pageSize:10,
            }
        },
        methods:{
            choseUnlockingWay(){
                console.log(this.unlockingWayValue)
            },
            choseunlockingResult(){
                console.log(this.unlockingResultValue)
            },
            handleCurrentChange(currentPage){

            }
        }
    }
</script>
<style scoped>
    /* .el-main {
      width: 100%;
      height: 100%;
      margin-top: 1.125rem;
      padding-top: 0;
      padding-bottom: 14.25rem;
      background-color: #fff;
    } */
    /* .el-form {
        padding: 0;
        width: 100%;
        padding: 0;
    }
    .el-form-item {
        display: inline-block;
        margin-bottom: 22px;
    } */
     /* 分页 */
    /* .el-pagination {
        position: relative;
        right: 0;
        padding-top: 60px;
        left: 0;
        text-align: center;
    }
    .el-pager li, .el-pagination button:disabled, .el-pagination .btn-next, .el-pagination .btn-prev{
        background-color: #E9EEF3;
    } */
</style>
