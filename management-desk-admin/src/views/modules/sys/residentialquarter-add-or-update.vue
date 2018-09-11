<template>
  <el-dialog :title="!dataForm.id ? '新增' : '修改'" :close-on-click-modal="false" :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
       <el-form-item label="所属社区" prop="cityId" style="display:inline-block;">
        <el-select v-model="dataForm.cityId" clearable placeholder="请选择区" @change="choseArea">
          <el-option v-for="item in area" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="subdistrictCode" style="display:inline-block" label-width="0">
        <el-select v-model="dataForm.subdistrictCode" clearable placeholder="请选择街道" @change="choseStreet">
          <el-option v-for="item in street" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="communityCode" style="display:inline-block" label-width="0">
        <!-- <el-input v-model="dataForm.communityCode" placeholder="所属社区编号"></el-input> -->
        <el-select v-model="dataForm.communityCode" clearable placeholder="请选择社区">
          <el-option v-for="item in community" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      
      <el-form-item label="名称" prop="name">
        <el-input v-model="dataForm.name" placeholder="小区名称"></el-input>
      </el-form-item>
      <el-form-item label="总栋数" prop="totalBuilding">
        <el-input v-model="dataForm.totalBuilding" placeholder="总栋数"></el-input>
      </el-form-item>
      <el-form-item label="地址" prop="address">
        <el-input v-model="dataForm.address" placeholder="地址"></el-input>
      </el-form-item>
      <el-form-item label="物业公司" prop="propertyCompany">
        <el-input v-model="dataForm.propertyCompany" placeholder="物业公司"></el-input>
      </el-form-item>
      <el-form-item label="建造时间" prop="constructionTime">
        <el-input v-model="dataForm.constructionTime" placeholder="建造时间"></el-input>
      </el-form-item>
      <el-form-item label="开发商" prop="constuctionCompany">
        <el-input v-model="dataForm.constuctionCompany" placeholder="开发商"></el-input>
      </el-form-item>
      <el-form-item label="建筑面积" prop="floorArea">
        <el-input v-model="dataForm.floorArea" placeholder="建筑面积，单位平方米"></el-input>
      </el-form-item>
      <el-form-item label="简介" prop="description">
        <el-input v-model="dataForm.description" placeholder="小区简介"></el-input>
      </el-form-item>
     
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      visible: false,
      dataForm: {
        id: 0,
        name: "",
        communityCode: "",
        totalBuilding: "",
        address: "",
        propertyCompany: "",
        constructionTime: "",
        constuctionCompany: "",
        floorArea: "",
        description: "",
        cityId: "",
        createTime: "",
        modifyTime: "",
        subdistrictCode:""
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
      street: [], //街道
      community: [], //社区
      dataRule: {
        name: [{required: true, message: "小区名称不能为空", trigger: "blur"}],
        cityId: [{required: true, message: "请选择区", trigger: "blur"}],
        subdistrictCode: [{required: true, message: "请选择街道", trigger: "blur"}],
        communityCode: [{ required: true, message: "所属社区编号不能为空", trigger: "blur"}],
        address: [{ required: true, message: "地址不能为空", trigger: "blur" }]
      }
    };
  },
  methods: {
     //获取街道代码
    choseArea(id) {
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/sys/subdistrict/list`),
        method: "get",
        params: this.$http.adornParams({
          code: this.dataForm.cityId
        })
      })
        .then(data => {
          this.dataForm.subdistrictCode="";
          this.dataForm.communityCode="";
          if (data.data.resultCode == 1000) {
            this.street = data.data.page.list;
            if(id.length > 6) {
              this.dataForm.subdistrictCode = id; //修改时显示房屋所在街道
            }
          }
        })
        .catch(function(err) {
          console.log("失败");
        });
    },
    //获取社区编码
    choseStreet(id) {
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/sys/community/list`),
        method: "get",
        params: this.$http.adornParams({
          code: this.dataForm.subdistrictCode
        })
      })
        .then(data => {
          this.dataForm.communityCode="";
          if (data.data.resultCode == 1000) {
            this.community = data.data.page.list;
            if(id.length > 9) {
              this.dataForm.communityCode = id; //修改时显示房屋所在社区
            }
          }
        })
        .catch(function(err) {
          console.log("失败");
        });
    },
    init(id) {
      this.dataForm.id = id || 0;
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.$http({
            url: this.$http.adornUrl(
              `/sys/residentialquarter/info/${this.dataForm.id}`
            ),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.name = data.residentialquarter.name;
              this.dataForm.totalBuilding = data.residentialquarter.totalBuilding;
              this.dataForm.address = data.residentialquarter.address;
              this.dataForm.propertyCompany = data.residentialquarter.propertyCompany;
              this.dataForm.constructionTime = data.residentialquarter.constructionTime;
              this.dataForm.constuctionCompany = data.residentialquarter.constuctionCompany;
              this.dataForm.floorArea = data.residentialquarter.floorArea;
              this.dataForm.description = data.residentialquarter.description;
              this.dataForm.cityId = data.residentialquarter.cityId;
              this.dataForm.subdistrictCode = data.residentialquarter.communityCode.substr(0, 9);
              this.choseArea(data.residentialquarter.communityCode.slice(0,9));//调用choseArea获取房屋所在街道
              this.dataForm.communityCode = data.residentialquarter.communityCode;
              this.choseStreet(data.residentialquarter.communityCode);//调用choseStreet获取房屋所在社区
              this.dataForm.createTime = data.residentialquarter.createTime;
              this.dataForm.modifyTime = data.residentialquarter.modifyTime;
            }
          });
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(
              `/sys/residentialquarter/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              name: this.dataForm.name,
              communityCode: this.dataForm.communityCode,
              totalBuilding: this.dataForm.totalBuilding,
              address: this.dataForm.address,
              propertyCompany: this.dataForm.propertyCompany,
              constructionTime: this.dataForm.constructionTime,
              constuctionCompany: this.dataForm.constuctionCompany,
              floorArea: this.dataForm.floorArea,
              description: this.dataForm.description,
              cityId: this.dataForm.cityId,
              createTime: this.dataForm.id === 0 ? new Date().getTime() : null,
              modifyTime: new Date().getTime()
            })
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.$message({
                message: "操作成功",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");
                }
              });
            } else {
              this.$message.error(data.msg);
            }
          });
        }
      });
    }
  }
};
</script>
