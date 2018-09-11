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
      <el-table-column align="center" header-align="center" prop="name" label="姓名">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="phone" label="电话">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="shequ" label="社区">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="address" label="地址">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="state" label="状态">
        <template slot-scope="scope">
          <el-popover trigger="hover" placement="top">
            <p>姓名: {{ scope.row.name }}</p>
            <p>说明: 该住户已经{{ scope.row.tody }}天未开门 </p>
            <div slot="reference" class="name-wrapper">
              <el-tag type="danger" size="medium">{{ scope.row.state }}</el-tag>
            </div>
          </el-popover>
        </template>
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="jg" label="说明">
        <template slot-scope="scope">
          <p>住户已经{{ scope.row.tody }}天未开门 </p>
        </template>
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="cjTime" label="创建时间">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="xgTime" label="修改时间">
      </el-table-column>
      <el-table-column align="center" header-align="center" label="操作">
        <template slot-scope="scope">
          <el-button v-show="scope.row.cl" @click="handleClick(scope.row)" type="text" size="small">处理</el-button>
          <el-button v-show="!scope.row.cl" type="text" size="small">已处理</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="block">
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage4" :page-sizes="[10, 20, 30, 40]" :page-size="6" layout="total, sizes, prev, pager, next, jumper" :total="6">
      </el-pagination>
    </div>
  </div>
</template>  
    
  <script>
export default {
  name: "home",
  data() {
    return {
      CL: true,
      imageUrl: "",
      value13: [],
      form: {
        name: "",
        region: "",
        type: []
      },
      currentPage4: 1,
      tableData2: [
        // {
        //   cjTime: "2016-05-02 10:35",
        //   xgTime: "2016-05-02 14:00",
        //   name: "王凯",
        //   phone: "13453738166",
        //   shequ: "杨家牌楼社区",
        //   address: " 杨家牌楼305号502室",
        //   cjr: "朱强",
        //   shr: "姚俊健",
        //   state: "异常",
        //   jg: "审核中",
        //   tody: 3,
        //   cl: true
        // },
        // {
        //   cjTime: "2016-05-02 10:35",
        //   xgTime: "2016-05-02 14:00",
        //   name: "朱强",
        //   phone: "13453738166",
        //   shequ: "东岳社区",
        //   address: "  西湖家园40幢6单元502室",
        //   cjr: "周龙",
        //   shr: "王虎",
        //   state: "异常",
        //   jg: "审核完",
        //   tody: 5,
        //   cl: true
        // },
        // {
        //   cjTime: "2016-05-02 10:35",
        //   xgTime: "2016-05-02 14:00",
        //   name: "姚俊健",
        //   phone: "13453738166",
        //   shequ: "留下社区",
        //   address: " 留和家苑15幢2单元302室",
        //   cjr: "赵强",
        //   shr: "姚俊健",
        //   state: "异常",
        //   jg: "审核完",
        //   tody: 7,
        //   cl: true
        // },
        // {
        //   cjTime: "2016-05-02 10:35",
        //   xgTime: "2016-05-02 14:00",
        //   name: "李树生",
        //   phone: "13453738166",
        //   shequ: "留下社区",
        //   address: " 茶市新村20幢1单元401室",
        //   cjr: "朱强",
        //   shr: "姚俊健",
        //   state: "异常",
        //   jg: "审核中",
        //   tody: 6,
        //   cl: false
        // },
        // {
        //   cjTime: "2016-05-02 10:35",
        //   xgTime: "2016-05-02 14:00",
        //   name: "钟鹏",
        //   phone: "13453738166",
        //   shequ: "留下社区",
        //   address: " 留和家苑15幢2单元302室",
        //   cjr: "朱强",
        //   shr: "姚俊健",
        //   state: "异常",
        //   jg: "已驳回",
        //   tody: 10,
        //   cl: false
        // },
        // {
        //   cjTime: "2016-05-02 10:35",
        //   xgTime: "2016-05-02 14:00",
        //   name: "成飞",
        //   phone: "13453738166",
        //   shequ: "留下社区",
        //   address: "留下花苑40幢6单元502室",
        //   cjr: "朱强",
        //   shr: "姚俊健",
        //   state: "异常",
        //   jg: "已通过",
        //   tody: 8,
        //   cl: true
        // }
      ]
    };
  },
  methods: {
    handleClick(row) {
      row.cl = !row.cl;
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
    padding-bottom: 14.25rem;
    background-color: #fff;
  }
  .el-form {
    padding: 0;
    height: 100px;
    line-height: 100px;
    text-align: left;
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