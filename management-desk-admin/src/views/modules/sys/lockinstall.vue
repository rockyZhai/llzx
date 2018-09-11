<template>
  <div class="mod-config">
    <el-form ref="form" :model="form" :inline="true" @keyup.enter.native="list()">
      <el-form-item>
        <el-select v-model="dataForm.areaValue" clearable placeholder="请选择区" @change="choseArea">
          <el-option v-for="item in area" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-select v-model="dataForm.streetValue" clearable placeholder="请选择街道" @change="choseStreet">
          <el-option v-for="item in street" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-select v-model="dataForm.communityCode" clearable placeholder="请选择社区">
          <el-option v-for="item in community" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.address" placeholder="详细地址"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.road" placeholder="道路名称"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.roadNumber" placeholder="道路编号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.buildingNumber" placeholder="幢号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.unitNumber" placeholder="单元号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.roomNumber" placeholder="房间号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-select v-model="dataForm.status" clearable placeholder="房源状态">
          <el-option value="1" label="待发布"></el-option>
          <el-option value="2" label="发布"></el-option>
        </el-select>
      </el-form-item>
    
      <el-form-item>
        <el-button @click="ChaXun">查询</el-button>
        <el-button type="primary" @click="addOrUpdateHandle()">新增</el-button>
      </el-form-item>
    </el-form>
    <el-table :data="tableData" style="width: 100%;" border v-loading="dataListLoading">
      <el-table-column align="center" header-align="center" prop="address" label="地址">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="departType" label="户型">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="payType" label="支付方式">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="towards" label="朝向">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="floorArea" label="面积">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="bathroomType" label="卫浴">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="status.title" label="状态">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="rent" label="租金">
      </el-table-column>
      <el-table-column align="center" header-align="center" label="操作">
        <template slot-scope="scope">
          <el-button type="text" @click="jump(scope.row)">详情</el-button>
          <el-button v-if="scope.row.status.value == 1" type="text" @click="release(scope.row)">发布</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination 
      @size-change="sizeChangeHandle" 
      @current-change="handleCurrentChange" 
      :current-page="currentPage" 
      :page-size="pageSize" 
      :page-sizes="[10, 20, 50, 100]" 
      layout="total, sizes, prev, pager, next, jumper" 
      :total="totalCount">
    </el-pagination>
    <!-- 新增 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="list"></add-or-update>
  </div>
</template>
<script>
import AddOrUpdate from "./lockinstall-add-or-update";
import { formatDate } from '@/utils/format'
export default {
  data() {
    return {
      dialogFormVisible: false,
      dataListLoading: false,
      addOrUpdateVisible: false,
      street: [], //街道
      community: [], //社区
      housingEstate: [], //小区
      dataForm: {
        areaValue: null, //区
        streetValue: null, //街道
        communityCode: null, //社区
        address:null,//详细地址
        road:null,//道路
        roadNumber:null,//道路编号
        buildingNumber:null,//幢号
        unitNumber:null,//单元号
        roomNumber:null,//房间号
        status:null,//状态
        housingEstateValue: null, //小区
        code:null//区  街道  社区编码
      },
      area: [
        { value: 330102, label: "上城区" },
        { value: 330103, label: "下城区" },
        { value: 330104, label: "江干区" },
        { value: 330105, label: "拱墅区" },
        { value: 330106, label: "西湖区" },
        { value: 330108, label: "滨江区" },
        { value: 330109, label: "萧山区" },
        { value: 330110, label: "余杭区" },
        { value: 330111, label: "富阳区" },
        { value: 330112, label: "临安区" },
        { value: 330122, label: "桐庐县" },
        { value: 330127, label: "淳安县" },
        { value: 330182, label: "建德市" }
      ],
      // 三级联动表单
      form: {
        value: 3301
      },
      addPersonForm: {
        name: "",
        phone: "",
        IdCard: "",
        houseAdress: "",
        community: "",
        area: ""
      },
      province: [],
      provinceValue: "",
      city: [],
      CityValue: "",
      block: [],
      blockValue: "",
      tableData: [], //table列表
      totalCount: 0,
      currentPage: 1,
      pageSize: 10,
      rules: {
        name: [
          { required: true, message: "请输入姓名", trigger: "blur" },
          { min: 2, max: 5, message: "长度在 2 到 5 个字符", trigger: "blur" }
        ],
        phone: [{ required: true, message: "请输入手机号", trigger: "blur" }],
        IdCard: [
          { required: true, message: "请输入身份证", trigger: "blur" },
          { min: 18, max: 18, message: "长度为 18 位", trigger: "blur" }
        ],
        houseAdress: [
          { required: true, message: "请输入房屋地址", trigger: "blur" }
        ],
        community: [
          { required: true, message: "请选择社区", trigger: "change" }
        ],
        area: [{ required: true, message: "请选择所属小区", trigger: "change" }]
      }
    };
  },
  components: {
    AddOrUpdate
  },//注册组件
  created() {
    var data = {
      pageCount: this.currentPage,
      pageSize: this.pageSize,
      city: this.form
    };
    this.list(data);
  },
  activated () {
    var data = {
      pageCount: this.currentPage,
      pageSize: this.pageSize,
      city: this.form
    };
    this.list(data);
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    //获取街道代码
    choseArea() {
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/sys/subdistrict/list`),
        method: "get",
        params: this.$http.adornParams({
          code: this.dataForm.areaValue
        })
      })
        .then(data => {
          this.dataForm.streetValue = ""; //街道
          this.dataForm.communityCode = ""; //社区
          if (data.data.resultCode == 1000) {
            this.street = data.data.page.list;
          }
        })
        .catch(function(err) {
          console.log("失败");
        });
    },
    //获取社区编码
    choseStreet() {
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/sys/community/list`),
        method: "get",
        params: this.$http.adornParams({
          code: this.dataForm.streetValue
        })
      })
        .then(data => {
          this.dataForm.communityCode = ""; //社区
          if (data.data.resultCode == 1000) {
            this.community = data.data.page.list;
          }
        })
        .catch(function(err) {
          console.log("失败");
        });
    },
    // 房屋列表
    list(data) {
      this.dataListLoading = true;
      var _this = this;
      if(data) {
        this.$http({
          url: this.$http.adornUrl(`/house/queryallstatus`),
          method: "post",
          data: this.$http.adornData(data)
        })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.tableData = data.data.resultData.apartments;
            this.totalCount = Number(data.data.resultData.num);
          }
          this.dataListLoading = false;
        })
        .catch(function(err) {
          console.log("失败");
          this.dataListLoading = false;
        })
      } else {
        var data = {
          pageCount: this.currentPage,
          pageSize: this.pageSize,
          city: this.form
        };
        this.list(data);
      }
    },
    // 翻页
    handleCurrentChange(currentPage) {
      this.currentPage = currentPage;
      var data = {
        pageCount: this.currentPage,
        pageSize: this.pageSize,
        city: this.form
      };
      this.list(data);
    },
    sizeChangeHandle(val) {
      this.pageSize = val;
      var data = {
        pageCount: this.currentPage,
        pageSize: this.pageSize,
        city: this.form
      };
      this.list(data);
    },
    // 查询
    ChaXun() {
      if(this.dataForm.areaValue && !this.dataForm.streetValue && !this.dataForm.communityCode){
        this.dataForm.code = this.dataForm.areaValue;
        var data = {
          address:this.dataForm.address,
          road:this.dataForm.road,
          roadNumber:this.dataForm.roadNumber,
          buildingNumber:this.dataForm.buildingNumber,
          unitNumber:this.dataForm.unitNumber,
          roomNumber:this.dataForm.roomNumber,
          status:this.dataForm.status,
          pageCount: 1,
          pageSize: this.pageSize,
          city: { value: this.dataForm.code}
        };
        console.log(data)
        this.list(data);
      }
      else if(this.dataForm.areaValue && this.dataForm.streetValue && !this.dataForm.communityCode){
        this.dataForm.code = this.dataForm.streetValue;
        var data = {
          address:this.dataForm.address,
          road:this.dataForm.road,
          roadNumber:this.dataForm.roadNumber,
          buildingNumber:this.dataForm.buildingNumber,
          unitNumber:this.dataForm.unitNumber,
          roomNumber:this.dataForm.roomNumber,
          status:this.dataForm.status,
          pageCount: 1,
          pageSize: this.pageSize,
          city: { value: this.dataForm.code}
        };
        console.log(data)
        this.list(data);
      }
      else {
        this.dataForm.code = this.dataForm.communityCode;
        var data = {
          address:this.dataForm.address,
          road:this.dataForm.road,
          roadNumber:this.dataForm.roadNumber,
          buildingNumber:this.dataForm.buildingNumber,
          unitNumber:this.dataForm.unitNumber,
          roomNumber:this.dataForm.roomNumber,
          status:this.dataForm.status,
          pageCount: 1,
          pageSize: this.pageSize,
          city: { value: this.dataForm.code}
        };
        console.log(data)
        this.list(data);
      }
    },
    // 携带参数跳转房屋详情
    jump(row) { 
      this.$router.push({ path: 'demo-fwxq'})
      this.$cookie.set('houseId', row.id)
    },
    //发布房源
    release(row) {
      const that = this;
      const userId = that.$cookie.get('userId');
      this.$http({
        url: this.$http.adornUrl(`/audit/do`),
        method: "post",
        data: this.$http.adornData({
          apartmentId: row.id,
          userId: userId
        })
      })
      .then(res => {
        if (res.data.resultCode == 1000) {
          var data = {
            pageCount: this.currentPage,
            pageSize: this.pageSize,
            city: this.form
          };
          this.$message({
            message: '发布成功了',
            type: 'success'
          });
          this.list(data);
        }else{
          this.$message.error(res.data.msg);
        }
      })
      .catch(function(err) {
        console.log(err);
      });
    },
    // 新增 / 修改
    addOrUpdateHandle (id) {
      this.addOrUpdateVisible = true
      this.$nextTick(() => {
        this.$refs.addOrUpdate.init(id)
      })
    },
  }
};
</script>
<style scoped>
.el-select {
  width: 185px;
}
</style>


