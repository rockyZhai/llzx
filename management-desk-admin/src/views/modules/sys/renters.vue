<template>
    <div class="mod-config">
        <el-form :inline="true" :model="dataForm">
            <el-form-item label="姓名">
                <el-input v-model="dataForm.name" placeholder="姓名" clearable></el-input>
            </el-form-item>
            <el-form-item label="电话">
                <el-input v-model="dataForm.phone" placeholder="电话" clearable></el-input>
            </el-form-item>
            <el-form-item label="身份证">
                <el-input v-model="dataForm.IDcard" placeholder="身份证" clearable></el-input>
            </el-form-item>
            <el-form-item>
                <el-button @click="ChaXun">查询</el-button>
            </el-form-item>
        </el-form>
        <el-table :data="dataList" border v-loading="dataListLoading" style="width: 100%;">
            <el-table-column prop="name" header-align="center" align="center" label="姓名">
            </el-table-column>
            <el-table-column prop="identityCard" header-align="center" align="center" label="身份证">
            </el-table-column>
            <el-table-column prop="phone" header-align="center" align="center" label="手机">
            </el-table-column>
            <el-table-column prop="birthday" :formatter="dateFormat" header-align="center" align="center" label="生日">
            </el-table-column>
            <el-table-column prop="nation.title" header-align="center" align="center" label="民族">
            </el-table-column>
            <el-table-column prop="sex.title" header-align="center" align="center" label="性别">
            </el-table-column>
            <el-table-column fixed="right" header-align="center" align="center" width="150" label="操作">
                <template slot-scope="scope">
                    <el-button type="text" @click="jump(scope.row)" size="small">详情</el-button>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination @size-change="sizeChangeHandle" @current-change="currentChangeHandle" :current-page="pageIndex" :page-sizes="[10, 20, 50, 100]" :page-size="pageSize" :total="totalCount" layout="total, sizes, prev, pager, next, jumper">
        </el-pagination>
    </div>
</template>
<script>
export default {
  data() {
    return {
      dataForm: {
        name: null,
        phone: null,
        IDcard: null,
      },
      dataList: [],
      pageIndex: 1,
      pageSize: 10,
      totalCount: 0,
      dataListLoading: false,//table  loding
    };
  },
  created() {
    var data = {
      page: this.pageIndex,
      pageSize: this.pageSize,
      type: 1,
      name: this.dataForm.name,
      identityCard: this.dataForm.IDcard,
      phone: this.dataForm.phone
    };
    this.getDataList(data);
  },
  methods: {
     //table列表数据格式化
    dateFormat (row, column) {
      var date = row.birthday.substr(0,4)+"年"+row.birthday.substr(4, 2)+'月'+row.birthday.substr(6, 2)+'日';
      return date
    },
    // 获取数据列表
    getDataList(data) {
      this.dataListLoading = true;
      this.$http({
        url: this.$http.adornUrl(`/person/query`),
        method: "post",
        data: this.$http.adornData(data)
      })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.dataList = data.data.resultData.persons;
            this.totalCount = Number(data.data.resultData.number);
          }
          this.dataListLoading = false;
        })
        .catch(function(err) {
          console.log("失败");
          this.dataListLoading = false;
        });
    },
    // 每页数
    sizeChangeHandle(val) {
      this.pageSize = val;
      var data = {
        page: this.pageIndex,
        pageSize: this.pageSize,
        type: 1,
        name: this.dataForm.name,
        identityCard: this.dataForm.IDcard,
        phone: this.dataForm.phone
      };
      console.log(this.pageSize);
      this.getDataList(data);
    },
    // 当前页
    currentChangeHandle(val) {
      this.pageIndex = val;
      var data = {
        page: this.pageIndex,
        pageSize: this.pageSize,
        type: 1,
        name: this.dataForm.name,
        identityCard: this.dataForm.IDcard,
        phone: this.dataForm.phone
      };
      this.getDataList(data);
    },
    // 查询
    ChaXun() {
      var data = {
        page: 1,
        pageSize: this.pageSize,
        type: 1,
        name: this.dataForm.name,
        identityCard: this.dataForm.IDcard,
        phone: this.dataForm.phone
      };
      this.getDataList(data);
    },
    //租客详情
    jump(row) {
      this.$cookie.set('id', row.id)//存cookie
      this.$router.push({ path: 'demo-rentersXQ'})
    },
  }
};
</script>
