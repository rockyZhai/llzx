<template>
  <el-dialog width="730px" :title="!dataForm.id ? '新增' : '修改'" :close-on-click-modal="false" :visible.sync="visible">
    <div class="twoOne" v-show="!dataForm.id"></div>
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="标题" prop="title" style="width:100%">
        <el-input v-model="dataForm.title" placeholder="标题"></el-input>
      </el-form-item>
      <el-form-item prop="areaValue" label="所在地区" style="display:inline-block;">
        <el-select v-model="dataForm.areaValue" clearable placeholder="请选择区" @change="choseArea">
          <el-option v-for="item in area" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label-width="0" prop="streetValue" style="display:inline-block;">
        <el-select v-model="dataForm.streetValue" clearable placeholder="请选择街道" @change="choseStreet()">
          <el-option v-for="item in street" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="communityCode" style="display:inline-block;" label-width="0">
        <el-select v-model="dataForm.communityCode" clearable placeholder="请选择社区" @change="choseCommunity()">
          <el-option v-for="item in community" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <p style="color:red;margin-top: 0;">* 请按实际情况填写详细地址</p>
          <el-form-item label="详细地址" prop="streetNumber" style="display:inline-block">
            <el-input v-model="dataForm.streetNumber" placeholder="如：文一西路" style="width:200px"></el-input>
          </el-form-item>
          <el-form-item prop="doorNumber" label-width="0" style="display:inline-block">
            <el-input v-model="dataForm.doorNumber" placeholder="如：588" style="width:185px"></el-input>号
          </el-form-item>
        <div>
          <el-form-item prop="housingEstateValue" style="display:inline-block;">
            <el-select v-model="dataForm.housingEstateValue" clearable placeholder="请选择小区">
              <el-option v-for="item in housingEstate" :key="item.id" :label="item.name" :value="item.id">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="buildingNumber" label-width="0" style="display:inline-block;">
            <el-input v-model="dataForm.buildingNumber" placeholder="如：7" style="width:85px"></el-input>幢
          </el-form-item>
          <el-form-item prop="unitNumber" style="display:inline-block;" label-width="0">
            <el-input v-model="dataForm.unitNumber" placeholder="如：1" style="width:85px"></el-input>单元
          </el-form-item>
          <el-form-item prop="roomNumber" label-width="0" style="display:inline-block;">
            <el-input v-model="dataForm.roomNumber" placeholder="如：601" style="width:75px"></el-input>室
          </el-form-item>
        </div>
      <!-- </div> -->
      <p>房屋基本信息：</p>
      <el-form-item label="租金" prop="rent" style="display:inline-block">
        <el-input type="number" v-model="dataForm.rent" placeholder="租金" style="width:263px"></el-input>
      </el-form-item>
      <el-form-item label="面积(㎡)" prop="proportion" style="display:inline-block">
        <el-input type="number" v-model="dataForm.proportion" placeholder="面积（㎡）" style="width:263px"></el-input>
      </el-form-item>
      <el-form-item label="电梯" prop="elevator" style="display:inline-block">
        <el-select v-model="dataForm.elevator" placeholder="请选择电梯" style="width:263px">
          <el-option v-for="item in dataForm.elevatorSelect" :key="item.value" :label="item.title" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="朝向" prop="towards" style="display:inline-block">
        <el-select v-model="dataForm.towards" placeholder="请选择朝向" style="width:263px">
          <el-option v-for="item in dataForm.towardsSelect" :key="item.value" :label="item.title" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="户型" prop="bedrooms" style="display:inline-block;">
        <el-input type="number" v-model="dataForm.bedrooms" style="width:265px" placeholder="室"></el-input>
        <span style="margin-left:10px">室</span>
      </el-form-item>
      <el-form-item prop="livingrooms" style="display:inline-block;" label-width="10px">
        <el-input type="number" v-model="dataForm.livingrooms" style="width:270px" placeholder="厅"></el-input>
        <span style="margin-left:10px">厅</span>
      </el-form-item>
      <el-form-item label="厨房" prop="kitchenType" style="display:inline-block;">
        <el-select v-model="dataForm.kitchenType" placeholder="请选择厨房类型" style="width:145px">
          <el-option v-for="item in dataForm.kitchenTypeSelect" :key="item.value" :label="item.title" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="kitchenrooms" style="display:inline-block;" label-width="0">
        <el-input type="number" v-model="dataForm.kitchenrooms" placeholder="个" style="width:100px"></el-input>
        <span>个</span>
      </el-form-item>
      <el-form-item label="卫生间" prop="bathroomType" style="display:inline-block;" label-width="70px">
        <el-select v-model="dataForm.bathroomType" placeholder="请选择卫生间类型" style="width:145px">
          <el-option v-for="item in dataForm.bathroomTypeSelect" :key="item.value" :label="item.title" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="bathrooms" style="display:inline-block;" label-width="0">
        <el-input type="number" v-model="dataForm.bathrooms" placeholder="个" style="width:100px"></el-input>
        <span>个</span>
      </el-form-item>

      <el-form-item label="装修" prop="decoration" style="display:inline-block;">
        <el-select v-model="dataForm.decoration" placeholder="请选择装修类型" style="width:263px">
          <el-option v-for="item in dataForm.decorationSelect" :key="item.value" :label="item.title" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      
      <el-form-item label="所在楼层" prop="floorNumber" style="display:inline-block;">
        <el-input type="number" v-model="dataForm.floorNumber" placeholder="所在楼层" style="width:263px"></el-input>
      </el-form-item>
      <el-form-item label="总楼层" prop="totalFloor" style="display:inline-block">
        <el-input type="number" v-model="dataForm.totalFloor" placeholder="总楼层" style="width:263px"></el-input>
      </el-form-item>
      <el-form-item label="建造年代" prop="constructionTime" style="display:inline-block;">
        <el-select v-model="dataForm.constructionTime" placeholder="请选择建造年代" style="width:263px">
          <el-option value="2018" label="2018"></el-option>
          <el-option value="2017" label="2017"></el-option>
          <el-option value="2016" label="2016"></el-option>
          <el-option value="2015" label="2015"></el-option>
          <el-option value="2014" label="2014"></el-option>
          <el-option value="2013" label="2013"></el-option>
          <el-option value="2012" label="2012"></el-option>
          <el-option value="2011" label="2011"></el-option>
          <el-option value="2010" label="2010"></el-option>
          <el-option value="2009" label="2009"></el-option>
          <el-option value="2008" label="2008"></el-option>
          <el-option value="2007" label="2007"></el-option>
          <el-option value="2006" label="2006"></el-option>
          <el-option value="2005" label="2005"></el-option>
          <el-option value="2004" label="2004"></el-option>
          <el-option value="2003" label="2003"></el-option>
          <el-option value="2002" label="2002"></el-option>
          <el-option value="2001" label="2001"></el-option>
          <el-option value="2000" label="2000"></el-option>
          <el-option value="1999" label="1999"></el-option>
          <el-option value="1998" label="1998"></el-option>
          <el-option value="1997" label="1997"></el-option>
          <el-option value="1996" label="1996"></el-option>
          <el-option value="1995" label="1995"></el-option>
          <el-option value="1994" label="1994"></el-option>
          <el-option value="1993" label="1993"></el-option>
          <el-option value="1992" label="1992"></el-option>
          <el-option value="1991" label="1991"></el-option>
          <el-option value="1990" label="1990"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="支付方式" prop="payType" style="display:inline-block">
        <el-select v-model="dataForm.payType" placeholder="请选择支付方式" style="width:263px">
          <el-option v-for="item in dataForm.payTypeSelect" :key="item.value" :label="item.title" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="出租方式" prop="rentType" style="display:inline-block">
        <el-select v-model="dataForm.rentType" placeholder="请选择出租方式" style="width:263px">
          <el-option v-for="item in dataForm.rentTypeSelect" :key="item.value" :label="item.title" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="简介" prop="intro">
        <el-input v-model="dataForm.intro" placeholder="简介"></el-input>
      </el-form-item>
      <!-- 房屋配置 -->
      <el-form-item label="房屋配置">
        <el-checkbox-group v-model="dataForm.furnitureCheckList" style="display:inline-block">
          <el-checkbox v-for="item in dataForm.furniture" :label="item.label" :key="item.label">{{item.value}}</el-checkbox>
        </el-checkbox-group>
        <el-checkbox-group v-model="dataForm.electricCheckList" style="display:inline-block">
          <el-checkbox v-for="item in dataForm.electric" :label="item.label" :key="item.label">{{item.value}}</el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      <!-- 房屋图片 -->
      <el-form-item label="房屋图片">
        <el-upload 
          action="http://www.iyuebanwan.com/easyrent/pub/uploadoss" 
          list-type="picture-card" 
          :on-preview="handlePictureCardPreview" 
          :on-remove="handleRemove" 
          :on-success="handleSuccess" 
          :limit="9" 
          :file-list="dataForm.fileList2"
          :on-exceed="handleExceed" 
          :before-upload="beforeAvatarUpload">
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dataForm.houseImg" alt="">
        </el-dialog>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit('dataForm')">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      detailA: true,
      visible: false,
      street: [], //街道
      community: [], //社区
      housingEstate: [], //小区
      dialogImageUrl: "",
      dialogVisible: false,
      URL: "https://gou.goucaiyun.net/",
      dataForm: {
        furnitureCheckList: [],
        electricCheckList: [],
        pictures: [],
        fileList2: [],//房屋图片数组
        title: null, //标题
        areaValue: null, //区
        streetValue: null, //街道
        communityCode: null, //社区
        housingEstateValue: null, //小区
        buildingNumber: null,//幢
        unitNumber: null,//单元
        roomNumber: null,//房间
        streetNumber: null,//街道
        doorNumber: null,//门牌号
        houseImg:null,//房屋图片
        id: 0,
        rent: null,
        proportion: null,
        bedrooms: null,
        livingrooms: null,
        elevator: null,//电梯
        elevatorSelect:[//电梯下拉框
          {title: "无",value:0},
          {title: "有",value:1}
        ],
        towards: null,//朝向
        towardsSelect:[//朝向下拉框
          {title: "南北",value:1},
          {title: "南",value:2},
          {title: "东",value:3},
          {title: "西",value:4},
          {title: "北",value:5}
        ],
        kitchenType: null,//厨房类型
        kitchenTypeSelect:[//厨房类型下拉框
          {title: "无",value:0},
          {title: "独立",value:1},
          {title: "公用",value:2},
        ],
        kitchenrooms: null,//厨房个数   
        bathroomType: null,//卫生间类型
        bathroomTypeSelect:[//卫生间类型下拉框
          {title: "独立",value:1},
          {title: "公用",value:2},
        ],
        bathrooms: null,//卫生间个数
        decoration: null,//装修类型
        decorationSelect:[//装修类型下拉框
          {title: "毛坯",value:1},
          {title: "简装",value:2},
          {title: "精装",value:3},
          {title: "豪华",value:4},
        ],
        floorNumber: null,
        totalFloor: null,
        constructionTime: null,
        payType: null,//支付类型
        payTypeSelect:[//支付类型下拉框
          {title: "无限制",value:0},
          {title: "月付",value:1},
          {title: "季付",value:2},
          {title: "年付",value:3}
        ],
        rentType: null,//出租方式
        rentTypeSelect: [//出租方式下拉框
          {title: "整租",value: 1},
          {title: "合租",value: 2}
        ],
        intro: null,
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
        title: [{ required: true, message: "标题不能为空", trigger: "blur" }],
        areaValue: [
          { required: true, message: "所属区域不能为空", trigger: "blur" }
        ],
        streetValue: [
          { required: true, message: "所属街道不能为空", trigger: "blur" }
        ],
        communityCode: [
          { required: true, message: "所属社区不能为空", trigger: "blur" }
        ],
      }
    };
  },
  methods: {
    //详细地址安小区添还是街道
    changeStatus: function(callback){
      this.detailA = callback;
    },
    //获取街道代码
    choseArea(id) {
      var _this = this;
      var value = this.dataForm.areaValue
      this.$http({
        url: this.$http.adornUrl(`/sys/subdistrict/list`),
        method: "get",
        params: this.$http.adornParams({
          code: value
        })
      })
      .then(data => {
        this.dataForm.streetValue = ""; //街道
        this.dataForm.communityCode = ""; //社区
        this.dataForm.housingEstateValue = "";//小区
        if (data.data.resultCode == 1000) {
          this.street = data.data.page.list;
          if(id.length > 6) {
            this.dataForm.streetValue = id; //修改是显示房屋所在街道
          }
        }
      })
      .catch(function(err) {
        console.log(err);
      });
    },
    //获取社区编码
    choseStreet(id,id2) {
      var _this = this;
      var value = this.dataForm.streetValue
      this.$http({
        url: this.$http.adornUrl(`/sys/community/list`),
        method: "get",
        params: this.$http.adornParams({
          code: value
        })
      })
      .then(data => {
        this.dataForm.communityCode = ""; //社区
        this.dataForm.housingEstateValue = "";//小区
        if (data.data.resultCode == 1000) {
          this.community = data.data.page.list;
          if(id.length > 9) {
            this.dataForm.communityCode = id; //修改是显示房屋所在街道
            this.dataForm.housingEstateValue = id2; //修改时显示房屋小区
          }
        }
      })
      .catch(function(err) {
        console.log(err);
      });
    },
    //获取小区编码
    choseCommunity(id) {
      var _this = this;
      var value = this.dataForm.communityCode
      console.log(id)
      this.$http({
        url: this.$http.adornUrl(`/sys/residentialquarter/list`),
        method: "get",
        params: this.$http.adornParams({
          code: value
        })
      })
      .then(data => {
        this.dataForm.housingEstateValue = "";//小区
        if (data.data.code == 0) {
          this.housingEstate = data.data.page.list;
          // if(id) {
          //   this.dataForm.housingEstateValue = id; //暂时别删
          // }
        }
      })
      .catch(function(err) {
        console.log("失败");
      });
    },
    //文件列表移除文件时的钩子
    handleRemove(file, fileList) {
      var pictur_Arr = [];
      for(var i in fileList) {
        var img2 = new Object();
        img2.key = fileList[i].name.key;
        pictur_Arr.push(img2) 
        if (i == fileList.length) break;
      }
      this.dataForm.pictures = pictur_Arr//房屋图片列表
    },
    //点击文件列表中已上传的文件时的钩子
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    //限制文件大小
    beforeAvatarUpload(file) {
      const isLt2M = file.size / 1024 / 1024 < 1;
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 1MB!");
      }
      return isLt2M;
    },
    //文件超出个数时限制的钩子
    handleExceed(file, fileList) {
      this.$message({
        message: "最多只能上传九张哦！",
        type: "warning"
      });
    },
    //图片上传成功处理
    handleSuccess(response, file, fileList) {
      if (response.resultCode == 1000) {
        for (var i = 0; i < response.resultData.length; i++) {
          var a = { key: response.resultData[i].title };
          this.dataForm.pictures.push(a);
          this.dataForm.houseImg = "https://gou.goucaiyun.net/"+response.resultData[i].title;
        }
      } else {
        this.$message.error("上传失败");
      }
    },

    init(id) {//修改
      this.dataForm.id = id || 0;
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          var data = {
            id: parseInt(this.dataForm.id)
          };
          this.$http({
            url: this.$http.adornUrl(`/house/detail`),
            method: "post",
            data: this.$http.adornData(data)
          })
          .then(({ data }) => {
            if (data && data.resultCode === 1000) {
              var JJ_Arr = [];
              var DQ_Arr = [];
              var IMG_Arr = [];
              var pictur_Arr = [];
              this.dataForm.title = data.resultData.title;
              this.dataForm.rent = data.resultData.rent;
              this.dataForm.proportion = data.resultData.floorArea;
              this.dataForm.communityCode = parseInt(data.resultData.communityCode);//社区
              this.dataForm.areaValue = parseInt(data.resultData.communityCode.slice(0,6));//区
              this.dataForm.streetValue = parseInt(data.resultData.communityCode.slice(0,9));//街道
              this.choseArea(data.resultData.communityCode.slice(0,9))
              this.choseStreet(data.resultData.communityCode.slice(0,12),data.resultData.rqId)
              this.choseCommunity(data.resultData.rqId)
              this.dataForm.housingEstateValue = data.resultData.rqId;//小区
              this.dataForm.buildingNumber = data.resultData.buildingNumber//幢号
              this.dataForm.unitNumber = data.resultData.unitNumber//单元号
              this.dataForm.roomNumber = data.resultData.roadNumber//房间号
              this.dataForm.elevator =  parseInt(data.resultData.elevator.value)
              this.dataForm.towards = parseInt(data.resultData.towards.value)
              this.dataForm.livingrooms = data.resultData.livingrooms
              this.dataForm.bedrooms = data.resultData.bedrooms
              this.dataForm.kitchenType = parseInt(data.resultData.kitchenType.value)
              this.dataForm.kitchenrooms = data.resultData.kichenrooms
              this.dataForm.bathroomType = parseInt(data.resultData.bathroomType.value)
              this.dataForm.bathrooms = data.resultData.bathrooms
              this.dataForm.decoration = parseInt(data.resultData.decoration.value)
              this.dataForm.floorNumber = data.resultData.floorNumber
              this.dataForm.totalFloor = data.resultData.totalFloor
              this.dataForm.payType = parseInt(data.resultData.payType.value)//支付方式
              this.dataForm.rentType = parseInt(data.resultData.rentType.value)//出租方式
              this.dataForm.constructionTime = data.resultData.constructionTime
              this.dataForm.intro = data.resultData.intro
              this.dataForm.streetNumber = data.resultData.road//路
              this.dataForm.doorNumber = data.resultData.roadNumber//路上的号
              for(var i in data.resultData.furniture) {
                JJ_Arr.push(data.resultData.furniture[i].value)
                if (i==data.resultData.furniture.length) break;
              }
              this.dataForm.furnitureCheckList = JJ_Arr//家具
              for(var i in data.resultData.electric) {
                DQ_Arr.push(data.resultData.electric[i].value)
                if (i==data.resultData.electric.length) break;
              }
              this.dataForm.electricCheckList = DQ_Arr//电器
              for(var i in data.resultData.pictures) {
                var img = new Object(), img2 = new Object();
                img.url = "https://gou.goucaiyun.net/"+ data.resultData.pictures[i].key;
                img.name = data.resultData.pictures[i];
                img2.key = data.resultData.pictures[i].key;
                IMG_Arr.push(img) 
                pictur_Arr.push(img2)
                if (i==data.resultData.pictures.length) break;
              }
              this.dataForm.fileList2 = IMG_Arr//房屋图片
              this.dataForm.pictures = pictur_Arr//房屋图片列表
            }
          });
        }
      });
    },
    // 表单提交
    dataFormSubmit(formName) {
      const that = this;
      const userId = that.$cookie.get('userId'); 
      var furnitureJson = [], electricJson = [];
      if (this.dataForm.furnitureCheckList !== undefined && this.dataForm.furnitureCheckList != null && this.dataForm.furnitureCheckList.length > 0) {
        for(var i= 0;i<this.dataForm.furnitureCheckList.length;i++){
          var furniture = new Object();
          furniture.value = this.dataForm.furnitureCheckList[i];
          furniture.title = '';
          furnitureJson.push(furniture);
        }
      }
      if (this.dataForm.electricCheckList !== undefined && this.dataForm.electricCheckList != null && this.dataForm.electricCheckList.length > 0) {
        console.log(this.dataForm.electricCheckList)
        for(var i= 0;i<this.dataForm.electricCheckList.length;i++){
          var electric = new Object();
          electric.value = this.dataForm.electricCheckList[i];
          electric.title = '';
          electricJson.push(electric);
        }
      }
      if(this.dataForm.id) {
        var data = {
          "id": this.dataForm.id,//修改时属性为id
          "communityCode": this.dataForm.communityCode,//社区编号
          "rqId": this.dataForm.housingEstateValue,//小区编号
          "buildingNumber": this.dataForm.buildingNumber,//幢号
          "unitNumber": this.dataForm.unitNumber,//单元号
          "roomNumber": this.dataForm.roomNumber,//房间号
          "floorNumber":this.dataForm.floorNumber,//楼层
          "totalFloor":this.dataForm.totalFloor,//总楼层
          "bedrooms":this.dataForm.bedrooms,//卧室数
          "livingrooms":this.dataForm.livingrooms,//厅数
          "bathrooms":this.dataForm.bathrooms,//浴室数
          "bathroomType": {
            value: this.dataForm.bathroomType,//浴室类型
            title: ""
          },
          "kitchenrooms":this.dataForm.kitchenrooms,//厨房数
          "kitchenType": {
            value: this.dataForm.kitchenType,//厨房类型
            title: ""
          },
          "floorArea":this.dataForm.proportion,//面积
          "decoration": {
            value: this.dataForm.decoration,//装修类型
            title: ""
          },
          "towards": {
            value: this.dataForm.towards,//朝向
            title: ""
          },
          "elevator": {
            value: this.dataForm.elevator,//电梯
            title: ""
          },
          "constructionTime":parseInt(this.dataForm.constructionTime),//建造时间（年即可）
          "title":this.dataForm.title,//标题
          "intro":this.dataForm.intro,//简介
          "rent":this.dataForm.rent,//租金
          "rentType": { 
            value: this.dataForm.rentType,//出租方式
            title: ""
          },
          "payType": {
            value: this.dataForm.payType,//支付方式
            title: ""
          },
          "entrustId":"",//申请号，为空则表示自建房源
          "pictures":this.dataForm.pictures,//房源图片
          "furniture":furnitureJson,//家具
          "electric":electricJson,//家电
          "createUid": userId,//创建人编号（管理员编号）
          "road":this.dataForm.streetNumber,//街路巷名称
          "roadNumber":this.dataForm.doorNumber//门牌号
        };
        console.log(data)
         console.log("新增")
      } else {
        var data = {
          "id": "",//修改时属性为空
          "communityCode": this.dataForm.communityCode,//社区编号
          "rqId": this.dataForm.housingEstateValue,//小区编号
          "buildingNumber": this.dataForm.buildingNumber,//幢号
          "unitNumber": this.dataForm.unitNumber,//单元号
          "roomNumber": this.dataForm.roomNumber,//房间号
          "floorNumber":this.dataForm.floorNumber,//楼层
          "totalFloor":this.dataForm.totalFloor,//总楼层
          "bedrooms":this.dataForm.bedrooms,//卧室数
          "livingrooms":this.dataForm.livingrooms,//厅数
          "bathrooms":this.dataForm.bathrooms,//浴室数
          "bathroomType": {
            value: this.dataForm.bathroomType,//浴室类型
            title: ""
          },
          "kitchenrooms":this.dataForm.kitchenrooms,//厨房数
          "kitchenType": {
            value: this.dataForm.kitchenType,//厨房类型
            title: ""
          },
          "floorArea":this.dataForm.proportion,//面积
           "decoration": {
            value: this.dataForm.decoration,//装修类型
            title: ""
          },
          "towards": {
            value: this.dataForm.towards,//朝向
            title: ""
          },
          "elevator": {
            value: this.dataForm.elevator,//电梯
            title: ""
          },
          "constructionTime":parseInt(this.dataForm.constructionTime),//建造时间（年即可）
          "title":this.dataForm.title,//标题
          "intro":this.dataForm.intro,//简介
          "rent":this.dataForm.rent,//租金
           "rentType": { 
            value: this.dataForm.rentType,//出租方式
            title: ""
          },
          "payType": {
            value: this.dataForm.payType,//支付方式
            title: ""
          },
          "entrustId":"",//申请号，为空则表示自建房源
          "pictures":this.dataForm.pictures,//房源图片
          "furniture":furnitureJson,//家具
          "electric":electricJson,//家电
          "createUid": userId,//创建人编号（管理员编号）
          "road":this.dataForm.streetNumber,//街路巷名称
          "roadNumber":this.dataForm.doorNumber//门牌号
        };
        console.log(data)
        console.log("修改")
      }
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(`/audit/updatedetail`),
            method: "post",
            data: this.$http.adornData(data)
          })
          .then(data => {
            if (data.data.resultCode == 1000) {
              console.log(data.data.resultData)
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.visible = false
                  this.$emit('refreshDataList')//刷新列表或者刷新详情页
                  this.dataForm.furnitureCheckList = [];
                  this.dataForm.electricCheckList = [];
                  this.dataForm.pictures = [];
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
          .catch(function(err) {
            console.log(err);
            // this.dataListLoading = false;
          });
        }
      });
    }
  }
};
</script>
<style scoped>
.el-checkbox + .el-checkbox {
  margin-left: 16px;
}
.el-upload-list--picture-card .el-upload-list__item {
  width: 75px;
  height: 75px;
}
.el-upload--picture-card {
  width: 75px;
  height: 75px;
  line-height: 85px;
}
.twoOne {
  text-align: center;
  margin-bottom: 10px;
}
</style>

