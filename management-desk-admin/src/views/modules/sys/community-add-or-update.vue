<template>
  <el-dialog :title="!dataForm.id ? '新增' : '修改'" :close-on-click-modal="false" :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="所属区" prop="cityId">
        <el-select v-model="dataForm.cityId" clearable placeholder="请选择区" @change="choseArea(null)" style="width:100%">
          <el-option v-for="item in area" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="所属街道" prop="subdistrictCode">
        <el-select v-model="dataForm.subdistrictCode" clearable placeholder="请选择街道" style="width:100%">
          <el-option v-for="item in street" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>

      <el-form-item label="社区编码" prop="code">
        <el-input v-model="dataForm.code" placeholder="社区编码"></el-input>
      </el-form-item>
      <el-form-item label="社区名称" prop="name">
        <el-input v-model="dataForm.name" placeholder="社区名称"></el-input>
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
        code: "",
        name: "",
        cityId: "",
        subdistrictCode: "",
        createTime: "",
        modifyTime: ""
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
      dataRule: {
        code: [
          { required: true, message: "社区编码不能为空", trigger: "blur" }
        ],
        name: [
          { required: true, message: "社区名称不能为空", trigger: "blur" }
        ],
        cityId: [
          { required: true, message: "所属城市不能为空", trigger: "blur" }
        ],
        subdistrictCode: [
          { required: true, message: "所属街道代码不能为空", trigger: "blur" }
        ],
        createTime: [
          { required: true, message: "创建时间不能为空", trigger: "blur" }
        ],
        modifyTime: [
          { required: true, message: "修改时间不能为空", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    //获取街道代码
    choseArea(subdistrictCode) {
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/sys/subdistrict/list`),
        method: "get",
        params: this.$http.adornParams({
          code: this.dataForm.cityId
        })
      }).then(data => {
        this.street = []
        this.dataForm.subdistrictCode = ''
        if (data.data.resultCode == 1000) {
          this.street = data.data.page.list;
          if (subdistrictCode !== undefined && subdistrictCode != null) {
            this.dataForm.subdistrictCode = subdistrictCode;
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
            url: this.$http.adornUrl(`/sys/community/info/${this.dataForm.id}`),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.code = data.community.code;
              this.dataForm.name = data.community.name;
              this.dataForm.cityId = data.community.cityId;
              this.dataForm.createTime = data.community.createTime;
              this.dataForm.modifyTime = data.community.modifyTime;
              var subdistrictCode = data.community.subdistrictCode;
              if (subdistrictCode === undefined || subdistrictCode == null) {
                if (this.dataForm.code !== undefined && this.dataForm.code != null && this.dataForm.code !=  '') {
                  subdistrictCode = this.dataForm.code.substring(0, 9)
                }
              }
              this.choseArea(subdistrictCode);
              
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
              `/sys/community/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              code: this.dataForm.code,
              name: this.dataForm.name,
              cityId: this.dataForm.cityId,
              subdistrictCode: this.dataForm.subdistrictCode,
              createTime:
                this.dataForm.id === 0 ? new Date().getTime() : undefined,
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
