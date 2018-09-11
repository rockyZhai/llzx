<template>
  <el-dialog :title="!dataForm.id ? '新增' : '修改'" :close-on-click-modal="false" :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="用户名" prop="userName">
        <el-input v-model="dataForm.userName" placeholder="登录帐号"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password" :class="{ 'is-required': !dataForm.id }">
        <el-input v-model="dataForm.password" type="password" placeholder="密码" :disabled="dataForm.id != 0 ? true : false"></el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="comfirmPassword" :class="{ 'is-required': !dataForm.id }">
        <el-input v-model="dataForm.comfirmPassword" type="password" placeholder="确认密码" :disabled="dataForm.id != 0 ? true : false"></el-input>
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="dataForm.email" placeholder="邮箱"></el-input>
      </el-form-item>
      <el-form-item label="手机号" prop="mobile">
        <el-input v-model="dataForm.mobile" placeholder="手机号"></el-input>
      </el-form-item>
      <el-form-item label="真实姓名" prop="realName">
        <el-input v-model="dataForm.realName" placeholder="真实姓名"></el-input>
      </el-form-item>
      <el-form-item label="工号" prop="empId">
        <el-input v-model="dataForm.empId" placeholder="工号"></el-input>
      </el-form-item>
      <el-form-item label="所属社区" prop="areaValue" style="display:inline-block">
        <el-select v-model="dataForm.areaValue" clearable placeholder="请选择区" @change="choseArea">
          <el-option v-for="item in area" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label-width="0" style="display:inline-block">
        <el-select v-model="dataForm.streetValue" clearable placeholder="请选择街道" @change="choseStreet">
          <el-option v-for="item in street" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label-width="0" style="display:inline-block">
        <el-select v-model="dataForm.communityCode" placeholder="请选择社区">
          <el-option v-for="item in communityList" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="职位" size="mini" prop="role">
        <el-radio-group v-model="dataForm.role">
          <el-radio :label="0">社工</el-radio>
          <el-radio :label="1">中介</el-radio>
          <el-radio :label="2">锁匠</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="角色" size="mini" prop="roleIdList">
        <el-checkbox-group v-model="dataForm.roleIdList">
          <el-checkbox v-for="role in roleList" :key="role.roleId" :label="role.roleId">{{ role.roleName }}</el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="状态" size="mini" prop="status">
        <el-radio-group v-model="dataForm.status">
          <el-radio :label="0">禁用</el-radio>
          <el-radio :label="1">正常</el-radio>
        </el-radio-group>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()" :loading="isLoading">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
import { isEmail, isMobile } from "@/utils/validate";
export default {
  data() {
    var validatePassword = (rule, value, callback) => {
      if (!this.dataForm.id && !/\S/.test(value)) {
        callback(new Error("密码不能为空"));
      } else {
        callback();
      }
    };
    var validateComfirmPassword = (rule, value, callback) => {
      if (!this.dataForm.id && !/\S/.test(value)) {
        callback(new Error("确认密码不能为空"));
      } else if (this.dataForm.password !== value) {
        callback(new Error("确认密码与密码输入不一致"));
      } else {
        callback();
      }
    };
    var validateEmail = (rule, value, callback) => {
      if (!isEmail(value)) {
        callback(new Error("邮箱格式错误"));
      } else {
        callback();
      }
    };
    var validateMobile = (rule, value, callback) => {
      if (!isMobile(value)) {
        callback(new Error("手机号格式错误"));
      } else {
        callback();
      }
    };
    return {
      communityList: [],//社区
      street: [], //街道
      dataListLoading: false,
      visible: false,
      isLoading: false,
      roleList: [],
      dataForm: {
        id: 0,
        userName: "",
        password: "",
        comfirmPassword: "",
        salt: "",
        email: "",
        mobile: "",
        realName: "",
        areaValue: null, //区
        streetValue: null, //街道
        communityCode: "", //社区
        empId: "",
        role: 0,
        roleIdList: [],
        status: 1
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
      dataRule: {
        userName: [
          { required: true, message: "用户名不能为空", trigger: "blur" }
        ],
        password: [{ validator: validatePassword, trigger: "blur" }],
        comfirmPassword: [
          { validator: validateComfirmPassword, trigger: "blur" }
        ],
        email: [
          { required: true, message: "邮箱不能为空", trigger: "blur" },
          { validator: validateEmail, trigger: "blur" }
        ],
        mobile: [
          { required: true, message: "手机号不能为空", trigger: "blur" },
          { validator: validateMobile, trigger: "blur" }
        ],
        areaValue:[
          { required: true, message: "请选择所属区域", trigger: "blur" },
        ],
        streetValue:[
          { required: true, message: "请选择所属街道", trigger: "blur" },
        ],
        communityCode:[
          { required: true, message: "请选择所属社区", trigger: "blur" },
        ],
      }
    };
  },
  methods: {
    init(id) {
      this.dataForm.id = id || 0;
      this.$http({
        url: this.$http.adornUrl("/sys/role/select"),
        method: "get",
        params: this.$http.adornParams()
      })
        .then(({ data }) => {
          this.roleList = data && data.code === 0 ? data.list : [];
        })
        .then(() => {
          this.visible = true;
          this.$nextTick(() => {
            this.$refs["dataForm"].resetFields();
          });
        })
        .then(() => {
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/sys/user/info/${this.dataForm.id}`),
              method: "get",
              params: this.$http.adornParams()
            }).then(({ data }) => {
              if (data && data.code === 0) {
                this.dataForm.userName = data.user.username;
                this.dataForm.salt = data.user.salt;
                this.dataForm.email = data.user.email;
                this.dataForm.mobile = data.user.mobile;
                this.dataForm.realName = data.user.realName;
                if(data.user.communityCode==null){
                  this.dataForm.areaValue = "";//区
                  this.dataForm.streetValue = ""; //街道
                  this.dataForm.communityCode = ""; //社区
                }else if(data.user.communityCode.length==6){
                  this.dataForm.areaValue = parseInt(data.user.communityCode.substr(0, 6));
                  this.choseArea(data.user.communityCode.slice(0,6));//调用choseArea获取房屋所在街道 
                  this.dataForm.streetValue = ""; //街道
                  this.dataForm.communityCode = ""; //社区
                }else if (data.user.communityCode.length==9) {
                  console.log(data.user.communityCode)
                  this.dataForm.communityCode = ""; //社区
                  this.dataForm.areaValue = parseInt(data.user.communityCode.substr(0, 6));
                  this.dataForm.streetValue = parseInt(data.user.communityCode.substr(0, 9));
                  console.log(this.dataForm.streetValue)
                  this.choseArea(data.user.communityCode.slice(0,9));//调用choseArea获取房屋所在街道 
                }else if(data.user.communityCode.length==12){
                  this.dataForm.areaValue = parseInt(data.user.communityCode.substr(0, 6));
                  this.dataForm.streetValue = parseInt(data.user.communityCode.substr(0, 9)); 
                  this.choseArea(data.user.communityCode.slice(0,9));//调用choseArea获取房屋所在街道
                  this.dataForm.communityCode = parseInt(data.user.communityCode); 
                  this.choseStreet(data.user.communityCode);//调用choseStreet获取房屋所在社区
                }
                this.dataForm.empId = data.user.empId;
                this.dataForm.role = data.user.role;
                this.dataForm.roleIdList = data.user.roleIdList;
                this.dataForm.status = data.user.status;
              }
            });
          }
        });
    },
    //获取街道代码
    choseArea(id) {
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
            if(id.length > 6) {
              this.dataForm.streetValue = id; //修改时显示所在街道
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
          code: this.dataForm.streetValue
        })
      })
        .then(data => {
          this.dataForm.communityCode = ""; //社区
          if (data.data.resultCode == 1000) {
            this.communityList = data.data.page.list;
            if(id.length > 9) {
              this.dataForm.communityCode = id; //修改时显示所在社区
            }
          }
        })
        .catch(function(err) {
          console.log("失败");
        });
    },
    // 表单提交
    dataFormSubmit() {
      var code = "";
      if(this.dataForm.areaValue && !this.dataForm.streetValue && !this.dataForm.communityCode){
        code = this.dataForm.areaValue;
      }else if(this.dataForm.areaValue && this.dataForm.streetValue && !this.dataForm.communityCode){
        code = this.dataForm.streetValue;
      }else {
        code = this.dataForm.communityCode;
      }
      console.log(this.dataForm.roleList);
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          this.isLoading = true;
          this.$http({
            url: this.$http.adornUrl(
              `/sys/user/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              userId: this.dataForm.id || undefined,
              username: this.dataForm.userName,
              password: this.dataForm.password,
              salt: this.dataForm.salt,
              email: this.dataForm.email,
              mobile: this.dataForm.mobile,
              realName: this.dataForm.realName,
              communityCode:code,
              empId: this.dataForm.empId,
              role: this.dataForm.role,
              status: this.dataForm.status,
              roleIdList: this.dataForm.roleIdList
            })
          }).then(({ data }) => {
            this.isLoading = false;
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
