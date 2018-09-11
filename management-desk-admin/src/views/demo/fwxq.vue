<template>
  <div class="Xq1">
    <el-main>
      <el-form>
        <el-form-item>
          <div style="position: relative;">
            <h2 class="Tit_h2">{{houseXQdata.title}}</h2>
            <el-button style="position: absolute;top: 1.25rem" type="primary" @click="addOrUpdateHandle(houseId)" plain>编辑</el-button>
          </div>
          <div class="Span3">
            <span>
              <div class="Span3-TOp">{{houseXQdata.rent}}元/月</div>
              <div class="Span3-Foot">房租</div>
            </span>
            <span>
              <div class="Span3-TOp">{{departTypeTitle}}</div>
              <div class="Span3-Foot">户型</div>
            </span>
            <span class="rightSpan" style="border-right: none">
              <div class="Span3-TOp">{{houseXQdata.floorArea}}平米</div>
              <div class="Span3-Foot">面积</div>
            </span>
          </div>
        </el-form-item>
        <el-form-item>
          <!-- 房屋详情 -->
          <div class="fouurDiv">
            <div class="fouurDiv_Tit">
              <img src="../../img/Fwgl_true.svg">
              <span class="tit">房屋详情</span>
              <span class="fouurDiv_Tit-Bj">
                <img src="../../img/bianji.svg">
                <el-button @click="dialogCq = true" class="edit" type="text">编辑产权</el-button>
              </span>
            </div>
            <div class="text item">装修： {{decorationTitle}}</div>
            <div class="text item">朝向： {{towardsTitle}}</div>
            <div class="text item">楼层： {{houseXQdata.floorNumber}}/{{houseXQdata.totalFloor}}</div>
            <div class="text item">年代： {{houseXQdata.constructionTime}}</div>
            <div class="text item">电梯： {{elevatorTitle}}</div>
            <div class="text item"> 产权：
              <el-popover>
                <img v-show="cqTp" :src="cqTp" style="width: 100%px;height: 100%;" />
                <img slot="reference" v-show="cqTp" :src="cqTp" style="width: 100px;height: 100px;" />
              </el-popover>
            </div>
          </div>
           <!-- 房源介绍 -->
          <div class="fouurDiv">
            <div class="fouurDiv_Tit">
              <img src="../../img/fyjs.png">
              <span class="tit">房源介绍</span>
              <!-- <span class="fouurDiv_Tit-Bj">
                <img src="../../img/bianji.svg">
                <el-button class="edit" type="text">编辑</el-button>
              </span> -->
            </div>
            <div class="fouurDiv_Cen3">{{houseXQdata.intro}}</div>
          </div>
        </el-form-item>
        <el-form-item>
          <!-- 房屋配置 -->
          <div class="fouurDiv">
            <div class="fouurDiv_Tit">
              <img src="../../img/housePZ.png">
              <span class="tit">房屋配置</span>
            </div>
            <div class="fouurDiv_Cen">
              <el-checkbox-group v-model="dataForm.furnitureCheckList" style="display:inline-block">
                <el-checkbox v-for="item in dataForm.furniture" :label="item.label" :key="item.label">{{item.value}}</el-checkbox>
              </el-checkbox-group>
              <el-checkbox-group v-model="dataForm.electricCheckList" style="display:inline-block;width: 37.5rem">
                <el-checkbox v-for="item in dataForm.electric" :label="item.label" :key="item.label">{{item.value}}</el-checkbox>
              </el-checkbox-group>
            </div>
          </div>
        </el-form-item>
      </el-form>
      <!-- 房东列表 -->
      <div class="fouurDiv_Tit">
        <img src="../../img/landlordXX.svg">
        <span class="tit" style="font-size:21px;">房东</span>
        <span class="fouurDiv_Tit-Bj">
          <img src="../../img/add.svg">
          <el-button class="edit" @click="dialogFormVisible = true" type="text" style="color:#333">添加房东</el-button>
        </span>
      </div>
      <el-table :data="landlords" style="width: 100%;" border v-loading="dataListLoading">
        <el-table-column align="center" header-align="center" prop="name" label="姓名">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="sex.title" label="性别">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="nation.title" label="民族">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="birthday" :formatter="dateFormat" label="出生">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="identityCard" label="身份证">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="phone" label="手机号码">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="status" label="状态">
          <template slot-scope="scope">
            <span v-if="scope.row.status === 0">正常</span>
            <span v-if="scope.row.status === 1">注销</span>
          </template>
        </el-table-column>
        <el-table-column align="center" header-align="center" label="操作">
          <template slot-scope="scope" v-if="scope.row.status===0">
            <el-button type="text" @click="editLandlords(scope.row)">编辑</el-button>
            <el-button type="text" @click="deleteLandlords(scope.row)">注销</el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 租客列表 -->
      <div class="fouurDiv_Tit">
        <img src="../../img/icon-renters.svg">
        <span class="tit" style="font-size:21px;">住户</span>
        <span class="fouurDiv_Tit-Bj">
          <img src="../../img/add.svg">
          <el-button class="edit" @click="addRentersVisible = true" type="text" style="color:#333">添加住户</el-button>
        </span>
      </div>
      <el-table :data="renters" style="width: 100%;" border v-loading="dataListLoading">
        <el-table-column align="center" header-align="center" prop="name" label="姓名">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="sex.title" label="性别">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="nation.title" label="民族">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="birthday" :formatter="dateFormat" label="出生">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="identityCard" width="180px" label="身份证">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="phone" label="手机号码">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="renterInTime" :formatter="dateFormatTime" label="起租时间">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="renterOutTime" :formatter="dateFormatTime" label="到期时间">
        </el-table-column>
        <el-table-column align="center" header-align="center" prop="status" label="状态">
          <template slot-scope="scope">
            <span v-if="scope.row.status === 0">正常</span>
            <span v-if="scope.row.status === 1">注销</span>
          </template>
        </el-table-column>
        <el-table-column align="center" header-align="center" label="操作">
          <template slot-scope="scope" v-if="scope.row.status===0"> 
            <el-button type="text" @click="editRenters(scope.row)">编辑</el-button>
            <el-button type="text" @click="deleteRenters(scope.row)">注销</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-main>
    <!-- 新增 房东信息 -->
    <el-dialog title="新增" :close-on-click-modal="false" :visible.sync="dialogFormVisible">
      <el-form :rules="rules" :model="addRentersForm" ref="addRentersForm" label-width="100px">
        <el-form-item label="姓名" prop="name">
          <el-input v-model="addRentersForm.name"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="sex">
          <el-radio-group v-model="addRentersForm.sex">
            <el-radio :label="0">男</el-radio>
            <el-radio :label="1">女</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="民族" prop="nation">
          <el-select v-model="addRentersForm.nation" placeholder="民族">
            <el-option v-for="item in nation" :key="item.value" :label="item.title" :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="出生" prop="birthday">
          <el-date-picker v-model="addRentersForm.birthday" type="date" placeholder="出生日期" format="yyyy 年 MM 月 dd 日" value-format="yyyyMMdd">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="身份证" prop="IdCard">
          <el-input v-model="addRentersForm.IdCard"></el-input>
        </el-form-item>
        <el-form-item label="手机号" prop="phone">
          <el-input v-model="addRentersForm.phone"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer" style="text-align:right">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addLandlordsSubmitForm('addRentersForm')">确 定</el-button>
      </div>
    </el-dialog>
    <!-- 修改 房东信息 -->
    <el-dialog title="修改" :close-on-click-modal="false" :visible.sync="editLandlordsVisible">
      <el-form :rules="rules" :model="editRentersForm" ref="editRentersForm" label-width="100px">
        <el-form-item label="姓名" prop="name">
          <el-input v-model="editRentersForm.name"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="sex">
          <el-radio-group v-model="editRentersForm.sex">
            <el-radio :label="0">男</el-radio>
            <el-radio :label="1">女</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="民族" prop="nation">
          <el-select v-model="editRentersForm.nation" placeholder="民族">
            <el-option v-for="item in nation" :key="item.value" :label="item.title" :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="出生" prop="birthday">
          <el-date-picker v-model="editRentersForm.birthday" type="date" placeholder="出生日期" format="yyyy 年 MM 月 dd 日" value-format="yyyyMMdd">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="身份证" prop="IdCard">
          <el-input v-model="editRentersForm.IdCard"></el-input>
        </el-form-item>
        <el-form-item label="手机号" prop="phone">
          <el-input v-model="editRentersForm.phone"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer" style="text-align:right">
        <el-button @click="editLandlordsVisible = false">取 消</el-button>
        <el-button type="primary" @click="editLandlordsSubmitForm('editRentersForm')">确 定</el-button>
      </div>
    </el-dialog>
    <!-- 编辑产权信息 -->
    <el-dialog title="产权信息" :close-on-click-modal="false" :visible.sync="dialogCq" center>
      <el-form class="Form" :model="cqForm">
        <el-form-item style="text-align: left;" label="产权图片" required>
          <el-upload action="http://www.iyuebanwan.com/easyrent/pub/uploadoss" list-type="picture-card" :on-preview="handleCqPreview" :on-remove="cqRemove" :on-success="cqSuccess" :limit="1" :on-exceed="cqExceed" :before-upload="beforeCqUpload">
            <i class="el-icon-plus"></i>
          </el-upload>
          <el-dialog :visible.sync="dialogVisibleCq">
            <img width="100%" :src="dialogCqUrl" alt="">
          </el-dialog>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer" style="text-align:right">
        <el-button @click="dialogCq = false">取 消</el-button>
        <el-button type="primary" @click="submitCq('cqForm')">确 定</el-button>
      </div>
    </el-dialog>
    <!-- 修改 住户信息 -->
    <el-dialog title="修改" :close-on-click-modal="false" :visible.sync="editRentersVisible">
      <el-form :rules="rules" :model="editRentersForm" ref="editRentersForm" label-width="100px">
        <el-form-item label="姓名" prop="name">
          <el-input v-model="editRentersForm.name"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="sex">
          <el-radio-group v-model="editRentersForm.sex">
            <el-radio :label="0">男</el-radio>
            <el-radio :label="1">女</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="民族" prop="nation">
          <el-select v-model="editRentersForm.nation" placeholder="民族">
            <el-option v-for="item in nation" :key="item.value" :label="item.title" :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="出生" prop="birthday">
          <el-date-picker v-model="editRentersForm.birthday" type="date" placeholder="出生日期" format="yyyy 年 MM 月 dd 日" value-format="yyyyMMdd">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="身份证" prop="IdCard">
          <el-input v-model="editRentersForm.IdCard"></el-input>
        </el-form-item>
        <el-form-item label="手机号" prop="phone">
          <el-input v-model="editRentersForm.phone"></el-input>
        </el-form-item>
        <el-form-item label="起租时间" prop="renterInTime">
          <el-date-picker v-model="editRentersForm.renterInTime" type="date" placeholder="起租时间" format="yyyy 年 MM 月 dd 日" value-format="timestamp">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="到期时间" prop="renterOutTime">
          <el-date-picker v-model="editRentersForm.renterOutTime" type="date" placeholder="到期时间" format="yyyy 年 MM 月 dd 日" value-format="timestamp">
          </el-date-picker>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer" style="text-align:right">
        <el-button @click="resetForm('editRentersForm')">取 消</el-button>
        <el-button type="primary" @click="editRentersSubmitForm('editRentersForm')">确 定</el-button>
      </div>
    </el-dialog>
    <!-- 新增 住户信息 -->
    <el-dialog title="新增" :close-on-click-modal="false" :visible.sync="addRentersVisible">
      <el-form :rules="rules" :model="addRentersForm" ref="addRentersForm" label-width="100px">
        <el-form-item label="姓名" prop="name">
          <el-input v-model="addRentersForm.name"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="sex">
          <el-radio-group v-model="addRentersForm.sex">
            <el-radio :label="0">男</el-radio>
            <el-radio :label="1">女</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="民族" prop="nation">
          <el-select v-model="addRentersForm.nation" placeholder="民族">
            <el-option v-for="item in nation" :key="item.value" :label="item.title" :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="出生" prop="birthday">
          <el-date-picker v-model="addRentersForm.birthday" type="date" placeholder="出生日期" format="yyyy 年 MM 月 dd 日" value-format="yyyyMMdd">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="身份证" prop="IdCard">
          <el-input v-model="addRentersForm.IdCard"></el-input>
        </el-form-item>
        <el-form-item label="手机号" prop="phone">
          <el-input v-model="addRentersForm.phone"></el-input>
        </el-form-item>
        <el-form-item label="起租时间" prop="renterInTime">
          <el-date-picker v-model="addRentersForm.renterInTime" type="date" placeholder="起租时间" format="yyyy 年 MM 月 dd 日" value-format="timestamp">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="到期时间" prop="renterOutTime">
          <el-date-picker v-model="addRentersForm.renterOutTime" type="date" placeholder="到期时间" format="yyyy 年 MM 月 dd 日" value-format="timestamp">
          </el-date-picker>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer" style="text-align:right">
        <el-button @click="resetFormAdd('addRentersForm')">取 消</el-button>
        <el-button type="primary" @click="addRentersSubmitForm('addRentersForm')">确 定</el-button>
      </div>
    </el-dialog>
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="Xq"></add-or-update>
  </div>
</template>
<script>
  import AddOrUpdate from "../modules/sys/lockinstall-add-or-update";
  import { formatDate } from '@/utils/format';
export default {
  data() {
    var identityCard = (rule, value, callback) => {
      var city = {
        11: "北京",12: "天津",13: "河北",14: "山西",15: "内蒙古",21: "辽宁",22: "吉林",
        23: "黑龙江 ",31: "上海",32: "江苏",33: "浙江",34: "安徽",35: "福建",36: "江西",
        37: "山东",41: "河南",42: "湖北 ",43: "湖南",44: "广东",45: "广西",46: "海南",
        50: "重庆",51: "四川",52: "贵州",53: "云南",54: "西藏 ",61: "陕西",62: "甘肃",
        63: "青海",64: "宁夏",65: "新疆",71: "台湾",81: "香港",82: "澳门",91: "国外"
      };
      var birthday =
        value.substr(6, 4) +
        "/" +
        Number(value.substr(10, 2)) +
        "/" +
        Number(value.substr(12, 2));
      var d = new Date(birthday);
      var newBirthday =
        d.getFullYear() +
        "/" +
        Number(d.getMonth() + 1) +
        "/" +
        Number(d.getDate());
      var currentTime = new Date().getTime();
      var time = d.getTime();
      var arrInt = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2];
      var arrCh = ["1", "0", "X", "9", "8", "7", "6", "5", "4", "3", "2"];
      var sum = 0,
        i,
        residue;
      for (i = 0; i < 17; i++) {
        sum += value.substr(i, 1) * arrInt[i];
      }
      residue = arrCh[sum % 11];
      if (!/^\d{17}(\d|x)$/i.test(value)) {
        this.$message.error("请输入正确的身份证号码");
        return false;
      } else if (city[value.substr(0, 2)] === undefined) {
        this.$message.error("身份证地区不合法");
        return false;
      } else if (time >= currentTime || birthday !== newBirthday) {
        this.$message.error("身份证生日不合法");
        return false;
      } else if (residue !== value.substr(17, 1)) {
        this.$message.error("非法身份证");
        return false;
      } else {
        callback();
      }
      return (
        city[value.substr(0, 2)] +
        "," +
        birthday +
        "," +
        (value.substr(16, 1) % 2 ? " 男" : "女")
      );
    };
    return {
      dataForm: {
        furnitureCheckList: [],
        electricCheckList: [],
        furniture: [
          { label: 10021, value: "床" },
          { label: 10022, value: "衣柜" },
          { label: 10023, value: "书桌" },
          { label: 10024, value: "椅子" },
          { label: 10025, value: "床头柜" }
        ],
        electric: [
          { label: 10011, value: "空调" },
          { label: 10012, value: "洗衣机" },
          { label: 10013, value: "冰箱" },
          { label: 10014, value: "电视" },
          { label: 10016, value: "网络" },
          { label: 10015, value: "热水器" },
          { label: 10017, value: "天然气" }
        ]
      },
      dataListLoading: false, //租客列表loding
      editRentersVisible: false, //修改模态框
      addRentersVisible: false, //新增模态框
      addOrUpdateVisible: false,
      dialogCq: false,
      cqImg: "",
      cqTp: "",
      dialogFormVisible: false, //新增房东模态框
      editLandlordsVisible: false, //修改房东模态框
      dialogVisible: false,
      dialogVisibleCq: false,
      dialogCq: false,
      fdModel: true,
      dialogImageUrl: "",
      dialogCqUrl: "",
      houseId: "",
      houseXQdata: "",
      departTypeTitle: "", //户型
      decorationTitle: "", //装修
      towardsTitle: "", //朝向
      kitchenTypeTitle: "", //厨房
      bathroomTypeTitle: "", //卫生间
      elevatorTitle: "", //电梯
      housePictures: [], //房屋照片
      URL: "https://gou.goucaiyun.net/",
      // 房屋配置复选
      electric: [], //家电
      furniture: [], //家具
      pictures: [],
      KTchecked: false,
      xyjchecked: false,
      bxchecked: false,
      dianshijichecked: false,
      rsqchecked: false,
      kuandaichecked: false,
      trqchecked: false,
      chuangchecked: false,
      yiguichecked: false,
      shuzhuochecked: false,
      yizichecked: false,
      ctgchecked: false,
      landlords: [], //房东列表
      renters: [], //房客列表
      nation: [],
      cqForm: {
        CqImg_r: ""
      },
      IdCardImg: "",
      house: {
        cq: ""
      },
      editRentersForm: {
        //修改表单
        id: "",
        name: "",
        sex: "",
        nation: "",
        birthday: "",
        phone: "",
        IdCard: "",
        renterInTime:null,
        renterOutTime:null
      },
      addRentersForm: {
        //新增表单
        id: "",
        name: "",
        sex: "",
        nation: "",
        birthday: "",
        phone: "",
        IdCard: "",
        renterInTime:null,
        renterOutTime:null
      },
      rules: {
        name: [
          { required: true, message: "请输入姓名", trigger: "blur" },
          { min: 2, max: 5, message: "长度在 2 到 5 个字符", trigger: "blur" }
        ],
        sex: [{ required: true, message: "请选择性别", trigger: "blur" }],
        nation: [{ required: true, message: "请选择民族", trigger: "blur" }],
        birthday: [
          { required: true, message: "请选择出生日期", trigger: "blur" }
        ],
        phone: [
          { required: true, message: "请输入手机号", trigger: "blur" },
          {
            pattern: /^1[3456789]\d{9}$/,
            message: "手机号码格式不正确",
            trigger: "blur"
          }
        ],
        IdCard: [
          {
            validator: identityCard,
            required: true,
            message: "请输入身份证",
            trigger: "blur"
          }
        ]
      }
    };
  },
  components: {
    AddOrUpdate
  },//注册组件
  activated() {
    this.Xq()
  },
  methods: {
    //进入详情页全部执行
    Xq() {
      this.houseId = this.$cookie.get("houseId");
      this.houseXQ();
      this.person();
      this.viewdata();
      this.nationSelect();
    },
    // 新增 / 修改
    addOrUpdateHandle (id) {
      const that = this;
      this.addOrUpdateVisible = true
      this.$nextTick(() => {
        this.$refs.addOrUpdate.init(that.houseId)
      })
    },
    //table列表数据格式化
    dateFormat(row, column) {
      var date =
        row.birthday.substr(0, 4) +
        "年" +
        row.birthday.substr(4, 2) +
        "月" +
        row.birthday.substr(6, 2) +
        "日";
      return date;
    },
    //table列表数据时间戳格式化
    dateFormatTime (row, column) {
        var date = row[column.property]
        if (date === undefined || date == null) {
          return ''
        }
        return formatDate(new Date(date), 'yyyy年MM月dd日')
      },
    //获取民族下拉列表
    nationSelect() {
      this.$http({
        url: this.$http.adornUrl(`/pub/dicdetail`),
        method: "post",
        data: this.$http.adornData({
          value: 1014
        })
      })
      .then(data => {
        if (data.data.resultCode == 1000) {
          this.nation = data.data.resultData;
        }
      })
      .catch(function(err) {
        console.log("失败");
      });
    },
    houseXQ() {
      var that = this;
      var data = {
        id: parseInt(this.houseId)
      };
      this.$http({
        url: this.$http.adornUrl(`/house/detail`),
        method: "post",
        data: this.$http.adornData(data)
      })
      .then(data => {
        if (data.data.resultCode == 1000) {
          var JJ_Arr = [];
          var DQ_Arr = [];
          this.houseXQdata = data.data.resultData;
          this.departTypeTitle = data.data.resultData.departType.title;
          this.decorationTitle = data.data.resultData.decoration.title;
          this.towardsTitle = data.data.resultData.towards.title;
          this.decorationTitle = data.data.resultData.decoration.title;
          this.kitchenTypeTitle = data.data.resultData.kitchenType.title;
          this.bathroomTypeTitle = data.data.resultData.bathroomType.title;
          this.elevatorTitle = data.data.resultData.elevator.title;
          this.housePictures = data.data.resultData.pictures;
          for(var i in data.data.resultData.furniture) {
              JJ_Arr.push(data.data.resultData.furniture[i].value)
              if (i==data.data.resultData.furniture.length) break;
            }
            this.dataForm.furnitureCheckList = JJ_Arr//家具
            for(var i in data.data.resultData.electric) {
              DQ_Arr.push(data.data.resultData.electric[i].value)
              if (i==data.data.resultData.electric.length) break;
            }
            this.dataForm.electricCheckList = DQ_Arr//电器
        }
      })
      .catch(function(err) {
        console.log(err);
      });
    },
    //获取房屋内人员信息（房东和租客）
    person() {
      this.$http({
        url: this.$http.adornUrl(`/person/querybyapartmentid`),
        method: "post",
        data: this.$http.adornData({
          apartmentId: parseInt(this.houseId)
        })
      })
      .then(data => {
        if (data.data.resultCode == 1000) {
          this.renters = data.data.resultData.renters;
          this.landlords = data.data.resultData.landlords;
        }
      })
      .catch(function(err) {
        console.log(err);
      });
    },
    //获取房屋消防和产权
    viewdata() {
      this.$http({
        url: this.$http.adornUrl(`/audit/viewdata`),
        method: "post",
        data: this.$http.adornData({
          id: this.houseId
        })
      })
      .then(data => {
        if (data.data.resultCode == 1000 && data.data.resultData.length > 0) {
          console.log(data.data.resultData)
          this.cqTp = data.data.resultData[0].url;
        }
      })
      .catch(function(err) {
        console.log(err);
      });
    },
    //身份证文件列表移除文件时的钩子
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    //身份证点击文件列表中已上传的文件时的钩子
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    //身份证限制文件大小
    beforeAvatarUpload(file) {
      const isLt2M = file.size / 1024 / 1024 < 1;
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 1MB!");
      }
      return isLt2M;
    },
    //身份证文件超出个数时限制的钩子
    handleExceed(file, fileList) {
      this.$message({
        message: "最多只能上传2张哦！",
        type: "warning"
      });
    },
    //身份证图片上传成功处理
    handleSuccess(response, file, fileList) {
      if (response.resultCode == 1000) {
        for (var i = 0; i < response.resultData.length; i++) {
          var a = { key: response.resultData[i].title };
          this.pictures.push(a);
        }
      } else {
        this.$message.error("上传失败");
      }
    },
    //产权文件列表移除文件时的钩子
    cqRemove(file, fileList) {
      console.log(file, fileList);
    },
    //产权点击文件列表中已上传的文件时的钩子
    handleCqPreview(file) {
      this.dialogCqUrl = file.url;
      this.dialogVisibleCq = true;
    },
    //产权限制文件大小
    beforeCqUpload(file) {
      const isLt2M = file.size / 1024 / 1024 < 1;
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 1MB!");
      }
      return isLt2M;
    },
    //产权文件超出个数时限制的钩子
    cqExceed(file, fileList) {
      this.$message({
        message: "最多只能上传1张哦！",
        type: "warning"
      });
    },
    //产权图片上传成功处理
    cqSuccess(response, file, fileList) {
      if (response.resultCode == 1000) {
        for (var i = 0; i < response.resultData.length; i++) {
          var a = { key: response.resultData[i].title };
          this.cqImg = this.URL + a.key;
        }
      } else {
        this.$message.error("上传失败");
      }
    },

    //提交产权图片
    submitCq(formName) {
      if (this.cqImg) {
        this.$http({
          url: this.$http.adornUrl(`/audit/uploaddata`),
          method: "post",
          data: this.$http.adornData({
            apartmentId: this.houseId,
            pictures: [
              {
                url: this.cqImg,
                type: 2
              }
            ]
          })
        })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.cqTp = this.cqImg;
            this.$message({
              message: "产权信息提交成功",
              type: "success",
              duration: 1500,
              onClose: () => {
                this.dialogCq = false;
              }
            });
          }
        })
        .catch(function(err) {
          console.log(err);
        });
      } else {
        this.$message({
          message: "请上传产权图片！",
          type: "warning"
        });
      }
    },
    // 注销住户
    deleteRenters(row) {
      this.$confirm("确定注销" + row.name + "吗?", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
      .then(() => {
        this.$http({
          url: this.$http.adornUrl(`/person/delete`),
          method: "post",
          data: this.$http.adornData({
            id: row.id, type: 1
          })
        })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.$message({
              message: "注销成功!",
              type: "success",
              duration: 1500,
              onClose: () => {
                this.person();
              }
            });
          }
        })
        .catch(function(err) {
          console.log(err);
        });
      })
      .catch(() => {
        console.log("已取消注销");
      });
    },
    // 修改 住户信息
    editRenters(row) {
      this.editRentersVisible = true;
      this.editRentersForm.id = row.id;
      this.editRentersForm.name = row.name;
      this.editRentersForm.sex = row.sex.value;
      this.editRentersForm.nation = row.nation.value;
      this.editRentersForm.birthday = row.birthday;
      this.editRentersForm.IdCard = row.identityCard;
      this.editRentersForm.phone = row.phone;
      this.editRentersForm.renterInTime = row.renterInTime;
      this.editRentersForm.renterOutTime = row.renterOutTime;
    },
    //编辑住户提交
    editRentersSubmitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(`/person/edit`),
            method: "post",
            data: this.$http.adornData({
              type: 1,
              name: this.editRentersForm.name,
              sex: parseInt(this.editRentersForm.sex),
              birthday: this.editRentersForm.birthday,
              nation: parseInt(this.editRentersForm.nation),
              identityCard: this.editRentersForm.IdCard,
              phone: this.editRentersForm.phone,
              id: parseInt(this.editRentersForm.id),
              apartmentId: parseInt(this.houseId),
              renterInTime: this.editRentersForm.renterInTime,
              renterOutTime: this.editRentersForm.renterOutTime
            })
          })
          .then(data => {
            if (data.data.resultCode == 1000) {
              this.$message({
                message: "编辑成功!",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.editRentersVisible = false;
                  this.$refs[formName].resetFields();
                  this.person();
                }
              });
            }
          })
          .catch(function(err) {
            console.log("编辑失败");
          });
        }
      });
    },
    // 修改住户取消按钮重置表单
    resetForm(formName) {
      this.editRentersVisible = false;
      this.$refs[formName].resetFields();
    },

    //新增住户提交
    addRentersSubmitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(`/person/add`),
            method: "post",
            data: this.$http.adornData({
              type: 1,
              name: this.addRentersForm.name,
              sex: parseInt(this.addRentersForm.sex),
              birthday: this.addRentersForm.birthday,
              nation: parseInt(this.addRentersForm.nation),
              identityCard: this.addRentersForm.IdCard,
              phone: this.addRentersForm.phone,
              apartmentId: parseInt(this.houseId),
              renterInTime:parseInt(this.addRentersForm.renterInTime),
              renterOutTime:parseInt(this.addRentersForm.renterOutTime),
              certificateType: 1
            })
          })
          .then(data => {
            if (data.data.resultCode == 1000) {
              this.$message({
                message: "添加成功!",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.addRentersVisible = false;
                  this.$refs[formName].resetFields();
                  this.person();
                }
              });
            }
          })
          .catch(function(err) {
            console.log("添加失败");
          });
        }
      });
    },
    // 新增住户取消按钮重置表单
    resetFormAdd(formName) {
      this.addRentersVisible = false;
      this.$refs[formName].resetFields();
    },

    //新增房东提交
    addLandlordsSubmitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(`/person/add`),
            method: "post",
            data: this.$http.adornData({
              type: 0,
              name: this.addRentersForm.name,
              sex: parseInt(this.addRentersForm.sex),
              birthday: this.addRentersForm.birthday,
              nation: parseInt(this.addRentersForm.nation),
              identityCard: this.addRentersForm.IdCard,
              phone: this.addRentersForm.phone,
              apartmentId: parseInt(this.houseId),
              certificateType: 1
            })
          })
          .then(data => {
            if (data.data.resultCode == 1000) {
              this.$message({
                message: "添加成功!",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.dialogFormVisible = false;
                  this.$refs[formName].resetFields();
                  this.person();
                }
              });
            }
          })
          .catch(function(err) {
            console.log("添加失败");
          });
        }
      });
    },
    // 修改 房东信息
    editLandlords(row) {
      this.editLandlordsVisible = true;
      this.editRentersForm.id = row.id;
      this.editRentersForm.name = row.name;
      this.editRentersForm.sex = row.sex.value;
      this.editRentersForm.nation = row.nation.value;
      this.editRentersForm.birthday = row.birthday;
      this.editRentersForm.IdCard = row.identityCard;
      this.editRentersForm.phone = row.phone;
    },
    //编辑房东提交
    editLandlordsSubmitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(`/person/edit`),
            method: "post",
            data: this.$http.adornData({
              type: 0,
              name: this.editRentersForm.name,
              sex: parseInt(this.editRentersForm.sex),
              birthday: this.editRentersForm.birthday,
              nation: parseInt(this.editRentersForm.nation),
              identityCard: this.editRentersForm.IdCard,
              phone: this.editRentersForm.phone,
              id: parseInt(this.editRentersForm.id)
            })
          })
          .then(data => {
            if (data.data.resultCode == 1000) {
              this.$message({
                message: "编辑成功!",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.editLandlordsVisible = false;
                  this.$refs[formName].resetFields();
                  this.person();
                }
              });
            }
          })
          .catch(function(err) {
            console.log("编辑失败");
          });
        }
      });
    },
    // 注销房东
    deleteLandlords(row) {
      this.$confirm("确定注销" + row.name + "吗?", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          this.$http({
            url: this.$http.adornUrl(`/person/delete`),
            method: "post",
            data: this.$http.adornData({
              id: row.id, type: 0
            })
          })
          .then(data => {
            if (data.data.resultCode == 1000) {
              this.$message({
                message: "注销成功!",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.person();
                }
              });
            }
          })
          .catch(function(err) {
            console.log(err);
          });
        })
        .catch(() => {
          console.log("已取消注销");
        });
    }
  }
};
</script>
<style scoped>
.Tit_h2 {
  display: inline-block;
  width: 18.75rem;
  margin-right: 3.75rem;
  overflow: hidden;
  text-overflow:ellipsis;
  white-space: nowrap;
}
.Span3 {
  margin-top: 16px;
  display: inline-block;
  height: 100px;
  background-color: #fffcf9;
  border: 1px solid #ffdfc0;
}
.Span3 > span {
  display: inline-block;
  width: 140px;
  height: 60px;
  border-right: 1px solid #ffdfc0;
  margin-top: 20px;
  text-align: center;
}
.Span3-TOp {
  font-size: 20px;
  color: #ff3333;
  font-weight: bold;
  height: 28px;
}
.Span3-Foot {
  font-size: 16px;
  color: #333333;
  margin-top: 6px;
}
.fouurDiv_Cen3 {
  display: inline-block;
  width: 420px;
  background-color: #f7f6fc;
  line-height: 30px;
  padding-left: 10px;
  border-left: 2px solid #ff9933;
}
.fouurDiv {
  width: 31.25rem;
  font-size: 16px;
  float: left;
  text-align: left;
}
.fouurDiv_Tit{
  margin-top: 36px;
}
.fouurDiv_Tit > img {
  width: 20px;
  height: 20px;
  vertical-align: text-bottom;
}
.fouurDiv_Tit > .tit {
  font-size: 17px;
  font-weight: bold;
  margin-left: 10px;
}
.fouurDiv_Tit-Bj {
  margin-left: 12px;
}
.fouurDiv_Tit-Bj > img {
  width: 20px;
  height: 20px;
}
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.fouurDiv_Cen ul li {
  display: inline-block;
}
.fouurDiv_Cen-Li2 {
  width: 120px;
}
.fouurDiv_Cen-Li2 img {
  margin-right: 5px;
  vertical-align: middle;
}
.edit {
  font-size: 16px;
  color: #999999;
}
.text {
  font-size: 16px;
  text-align: left;
}
</style>