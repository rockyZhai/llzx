<template>
    <div class="mod-config">
        <el-form ref="form" :model="form" :inline="true">
            <el-form-item label="设备名称">
                <el-input placeholder="设备名称" v-model="form.deviceName"></el-input>
            </el-form-item>
            <el-form-item label="设备序列号">
                <el-input placeholder="设备序列号" v-model="form.deviceSerialNumber"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button @click="queryDevice">查询</el-button>
            </el-form-item>
        </el-form>
        <el-table :data="tableData" v-loading="dataListLoading" style="width: 100%" border>
            <el-table-column align="center" header-align="center" prop="name" label="设备名称">
            </el-table-column>
            <el-table-column align="center" header-align="center" prop="sn" label="设备序列号">
            </el-table-column>
            <el-table-column align="center" header-align="center" label="房屋地址" prop="apartments">
                <template slot-scope="scope">
                    <div v-for=" (g,index) in scope.row.apartments" :key="index">
                        {{g.title}}
                    </div>
                </template>
            </el-table-column>
            <el-table-column align="center" header-align="center" prop="type.title" label="设备类型">
            </el-table-column>
            <el-table-column align="center" header-align="center" prop='isInstalled' label="设备状态">
                <template slot-scope="scope">
                    <span v-if="scope.row.isInstalled === 0">在库</span>
                    <span v-if="scope.row.isInstalled === 1">已安装</span>
                </template>
            </el-table-column>
            <el-table-column align="center" header-align="center" label="操作">
                <template slot-scope="scope">
                    <el-button v-show="!scope.row.dealt" type="text" @click="editKeyAudit(scope.row)">钥匙审核</el-button>
                    <el-button v-show="scope.row.dealt" type="text">已登记</el-button>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination 
        @size-change="sizeChangeHandle"
        @current-change="currentChangeHandle" 
        :current-page="currentPage" 
        :page-size="6" 
        :page-sizes="[10, 20, 50, 100]" 
        layout="total, sizes, prev, pager, next, jumper"
        :total="totalCount">
        </el-pagination>
    </div>
</template>
<script>
export default {
  data() {
    return {
      dataListLoading: false,
      visible2: false,
      KeyAudit: "钥匙审核",
      form: {
        deviceName: "",
        deviceSerialNumber: ""
      },
      tableData: [],
      totalCount: 0,
      currentPage: 1,
      pageSize: 10
    };
  },
  created() {
    var data = {
        page: this.currentPage,
        name: this.form.deviceName,
        sn: this.form.deviceSerialNumber,
        pageSize: this.pageSize
    };
    this.deviceList(data);
  },
  methods: {
    deviceList(data) {
      this.dataListLoading = true;
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/device/query`),
        method: "post",
        data: this.$http.adornData(data)
      })
    //   this.axios({
    //     url: "device/query",
    //     method: "post",
    //     data: data
    //   })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.tableData = data.data.resultData.devices;
            this.totalCount = Number(data.data.resultData.num);
          }
          this.dataListLoading = false;
        })
        .catch(function(err) {
          console.log("失败");
          this.dataListLoading = false;
        });
    },
    // 分页
    currentChangeHandle(val) {
        this.currentPage = val;
        var data = {
            page: this.currentPage,
            name: this.form.deviceName,
            sn: this.form.deviceSerialNumber,
            pageSize: this.pageSize
        };
        this.deviceList(data);
    },
    // 每页数
    sizeChangeHandle (val) {
        this.pageSize = val
        var data = {
            page: this.currentPage,
            name: this.form.deviceName,
            sn: this.form.deviceSerialNumber,
            pageSize: this.pageSize
        };
        console.log(this.pageSize)
        this.deviceList(data)
    },
    //查询
    queryDevice(){
        var data = {
            page: 1,
            name: this.form.deviceName,
            sn: this.form.deviceSerialNumber,
            pageSize: this.pageSize
        };
        this.deviceList(data);
    },
    editKeyAudit(row) {
      row.dealt = !row.dealt;
    }
  }
};
</script>
<style scoped>
/* .el-main {
      width: 100%;
      height: 100%;
      margin-top: 1.125rem;
      padding-top: 0;
      padding-bottom: 14.25rem;
      background-color: #fff;
    }
   .el-form {
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