<template>
    <div class="mod-config">
        <el-form ref="form" :model="form" :inline="true">
            <el-form-item label="姓名">
                <el-input v-model="form.name" placeholder="姓名"></el-input>
            </el-form-item>
            <el-form-item label="审核结果">
                <el-select v-model="form.region" placeholder="请选择状态">
                    <el-option label="未审核" value="0"></el-option>
                    <el-option label="已通过" value="1"></el-option>
                    <el-option label="已驳回" value="1"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="创建时间">
                <el-date-picker v-model="value13" type="daterange" start-placeholder="开始日期" end-placeholder="结束日期" :default-time="['00:00:00', '23:59:59']">
                </el-date-picker>
            </el-form-item>
            <el-form-item>
                <el-button @click="onSubmit">查询</el-button>
            </el-form-item>
        </el-form>
        <el-table :data="tableData2" style="width: 100%;" border>
            <el-table-column 
              align="center" 
              header-align="center" 
              prop="name" 
              label="姓名">
            </el-table-column>
            <el-table-column 
              align="center" 
              header-align="center" 
              prop="phone" 
              label="电话">
            </el-table-column>
            <el-table-column 
              align="center" 
              header-align="center" 
              prop="shequ" 
              label="社区">
            </el-table-column>
            <el-table-column 
              align="center" 
              header-align="center" 
              prop="address" 
              label="地址">
            </el-table-column>
            <el-table-column 
              align="center" 
              header-align="center" 
              prop="cjTime"
              label="登记时间">
            </el-table-column>
            <el-table-column 
              align="center" 
              header-align="center" 
              prop="xgTime"
              label="操作">
                <template slot-scope="scope">
                    <el-button @click="handleClick(scope.row)" type="text" size="small">发送消息</el-button>
                </template>
            </el-table-column>
        </el-table>
        <div class="block">
            <el-pagination 
              @size-change="handleSizeChange" 
              @current-change="handleCurrentChange" 
              :current-page="currentPage4" 
              :page-sizes="[10, 20, 30, 40]" 
              :page-size="6" 
              layout="total, sizes, prev, pager, next, jumper" 
              :total="6">
            </el-pagination>
        </div>
    </div>
</template>  
    
  <script>
export default {
  name: "home",
  data() {
    return {
      imageUrl: "",
      value13: [],
      form: {
        name: "",
        region: "",
        type: []
      },
      currentPage4: 4,
      tableData2: [
        // {
        //   cjTime: "2018-05-02 10:35",
        //   xgTime: "2016-05-02 14:00",
        //   name: "董志龙",
        //   phone: "13453738166",
        //   shequ: "杨家牌楼社区",
        //   address: " 杨家牌楼301号501室",
        //   cjr: "朱强",
        //   shr: "姚俊健",
        //   state: "已提交审核",
        //   jg: "审核中"
        // },
        // {
        //   cjTime: "2018-05-02 10:40",
        //   xgTime: "2016-05-02 14:00",
        //   name: "顾超",
        //   phone: "13453738166",
        //   shequ: "东岳社区",
        //   address: " 西湖家园22幢3单元302室",
        //   cjr: "周龙",
        //   shr: "王虎",
        //   state: "已驳回审核",
        //   jg: "已审核"
        // },
        // {
        //   cjTime: "2018-05-07 10:30",
        //   xgTime: "2016-05-02 14:00",
        //   name: "翟媛媛",
        //   phone: "13453738166",
        //   shequ: "留下社区",
        //   address: " 茶市新村20幢1单元401室",
        //   cjr: "赵强",
        //   shr: "姚俊健",
        //   state: "已通过",
        //   jg: "已审核"
        // },
        // {
        //   cjTime: "2018-06-02 14:00",
        //   xgTime: "2016-05-02 14:00",
        //   name: "李树生",
        //   phone: "13453738166",
        //   shequ: "留下社区",
        //   address: " 留和家苑15幢2单元302室",
        //   cjr: "朱强",
        //   shr: "姚俊健",
        //   state: "已提交审核",
        //   jg: "已审核"
        // },
        // {
        //   cjTime: "2018-05-07 10:00",
        //   xgTime: "2016-05-02 14:00",
        //   name: "中鹏",
        //   phone: "13453738166",
        //   shequ: "东岳社区",
        //   address: "  西湖家园40幢6单元502室",
        //   cjr: "朱强",
        //   shr: "姚俊健",
        //   state: "已提交审核",
        //   jg: "已驳回"
        // },
        // {
        //   cjTime: "2018-07-02 15:00",
        //   xgTime: "2016-05-02 14:00",
        //   name: "成飞",
        //   phone: "13453738166",
        //   shequ: "杨家牌楼社区",
        //   address: " 杨家牌楼305号502室",
        //   cjr: "朱强",
        //   shr: "姚俊健",
        //   state: "已提交审核",
        //   jg: "已通过"
        // }
      ]
    };
  },
  methods: {
    handleClick(row) {
      this.$alert("确定给该用户发送相关消息吗？", "消息提醒", {
        confirmButtonText: "确定",
        callback: action => {
          if (action == "confirm") {
            this.$message({
              type: "success",
              message: `消息已发送`
            });
          } else {
            this.$message({
              type: "info",
              message: `已取消`
            });
          }
        }
      });
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    },
    onSubmit() {
      console.log("submit!");
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
    padding-bottom: 15.25rem;
    background-color: #fff;
  }
  .el-form {
    height: 100px;
    line-height: 100px;
    text-align: left;
    padding: 0;
  }
  .el-form-item {
    display: inline-block;
  }
  .avatar-uploader .el-upload {
      border: 1px dashed #d9d9d9;
      border-radius: 6px;
      cursor: pointer;
      position: relative;
      overflow: hidden;
    }
    .avatar-uploader .el-upload:hover {
      border-color: #409EFF;
    }
    .avatar-uploader-icon {
      font-size: 28px;
      color: #8c939d;
      width: 178px;
      height: 178px;
      line-height: 178px;
      text-align: center;
    }
    .avatar {
      width: 178px;
      height: 178px;
      display: block;
    }
    .el-table .warning-row {
      background: oldlace;
    }
  
    .el-table .success-row {
      background: #f0f9eb;
    }
    .el-table--enable-row-transition {
      background-color: #E9EEF3;
      margin: 20px auto;
    }
    .el-table__header-wrapper {
      line-height: 30px;
    }
    .is-leaf > div {
      text-align: center;
    } */
</style>