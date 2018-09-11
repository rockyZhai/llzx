<template>
    <div class="mod-config">
        <el-form class="landlordsInfo">
            <el-form-item class="landlordsInfo_title">
                <span>
                    <img src="../../img/icon-landlords.svg" class="landlordsInfo_icon">租客信息
                </span>
                <span class="landlordsInfo_sapn" @click="dialogFormVisible = true">
                    <img src="../../img/icon-bianji.svg" class="landlordsInfo_icon">编辑
                </span>
            </el-form-item>
        </el-form>
        <el-form class="landlordsInfo_data">
            <el-form-item label="姓名:">{{landlordsInfo.name}}</el-form-item>
            <el-form-item label="性别:">
                <el-radio-group v-model="landlordsInfoSexValue">
                    <el-radio :label="0">男</el-radio>
                    <el-radio :label="1">女</el-radio>
                    <!-- :disabled='isDisabled("1")' -->
                </el-radio-group>
            </el-form-item>
            <el-form-item label="民族:">{{landlordsInfoNation}}</el-form-item>
            <el-form-item label="出生:">{{landlordsInfoBirthday}}</el-form-item>
            <el-form-item label="身份证号码:">{{landlordsInfo.identityCard}}</el-form-item>
            <el-form-item label="手机号码:">{{landlordsInfo.phone}}</el-form-item>
        </el-form>

        <!-- 房东房屋列表 -->
        <p style="text-align: center;">我的房屋</p>
        <el-table :data="landlordApartments" border v-loading="dataListLoading" style="width: 100%;">
            <el-table-column prop="title" header-align="center" align="center" label="地址"></el-table-column>
            <el-table-column prop="departType" header-align="center" align="center" label="户型"></el-table-column>
            <el-table-column prop="towards" header-align="center" align="center" label="朝向"></el-table-column>
            <el-table-column prop="floorArea" header-align="center" align="center" label="面积（㎡）"></el-table-column>
            <el-table-column prop="livingrooms" header-align="center" align="center" label="客厅（个）"></el-table-column>
            <el-table-column prop="bedrooms" header-align="center" align="center" label="卧室（个）"></el-table-column>
            <el-table-column prop="kichenrooms" header-align="center" align="center" label="厨房（个）"></el-table-column>
            <el-table-column prop="bathrooms" header-align="center" align="center" label="卫浴（个）"></el-table-column>
            <el-table-column prop="bathroomType" header-align="center" align="center" label="卫浴类型"></el-table-column>
            <el-table-column prop="rent" header-align="center" align="center" label="租金"></el-table-column>
            <el-table-column prop="payType" header-align="center" align="center" label="支付方式"></el-table-column>
            <el-table-column prop="status.title" header-align="center" align="center" label="状态"></el-table-column>
        </el-table>

        <!-- 租客房屋列表 -->
        <p style="text-align: center;">我的租赁房屋</p>
        <el-table :data="renterApartments" border v-loading="dataListLoading" style="width: 100%;">
            <el-table-column prop="title" header-align="center" align="center" label="地址"></el-table-column>
            <el-table-column prop="departType" header-align="center" align="center" label="户型"></el-table-column>
            <el-table-column prop="towards" header-align="center" align="center" label="朝向"></el-table-column>
            <el-table-column prop="floorArea" header-align="center" align="center" label="面积（㎡）"></el-table-column>
            <el-table-column prop="livingrooms" header-align="center" align="center" label="客厅（个）"></el-table-column>
            <el-table-column prop="bedrooms" header-align="center" align="center" label="卧室（个）"></el-table-column>
            <el-table-column prop="kichenrooms" header-align="center" align="center" label="厨房（个）"></el-table-column>
            <el-table-column prop="bathrooms" header-align="center" align="center" label="卫浴（个）"></el-table-column>
            <el-table-column prop="bathroomType" header-align="center" align="center" label="卫浴类型"></el-table-column>
            <el-table-column prop="rent" header-align="center" align="center" label="租金"></el-table-column>
            <el-table-column prop="payType" header-align="center" align="center" label="支付方式"></el-table-column>
            <el-table-column prop="status.title" header-align="center" align="center" label="状态"></el-table-column>
        </el-table>

        <!-- 租客信息编辑 -->
        <el-dialog title='修改' :visible.sync="dialogFormVisible" :close-on-click-modal="false">
            <el-form :model="dataForm" :rules="dataFormRule" ref="dataForm"  label-width="150px">
                <el-form-item label="姓名" prop="name">
                    <el-input v-model="dataForm.name" placeholder="姓名"></el-input>
                </el-form-item>
                <el-form-item label="性别" prop="sex">
                    <el-radio-group v-model="dataForm.sex">
                        <el-radio :label="0">男</el-radio>
                        <el-radio :label="1">女</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="民族" prop="nation">
                    <el-select v-model="dataForm.nation" placeholder="民族" @change="changeNation">
                        <el-option v-for="item in nation" :key="item.value" :label="item.title" :value="item.value">
                        </el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="出生" prop="birthday">
                    <el-date-picker
                        v-model="dataForm.birthday"
                        type="date"
                        placeholder="出生日期"
                        format="yyyy 年 MM 月 dd 日"
                        value-format="yyyyMMdd">
                    </el-date-picker>
                </el-form-item>
                <el-form-item label="身份证号码" prop="identityCard">
                    <el-input v-model="dataForm.identityCard" placeholder="身份证号码"></el-input>
                </el-form-item>
                <el-form-item label="手机号码" prop="phone">
                    <el-input v-model="dataForm.phone" placeholder="手机号码"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="landlordsEditSure('dataForm')">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script>
export default {
    data(){
        var identityCard = (rule,value,callback)=>{
            var city = {11:"北京",12:"天津",13:"河北",14:"山西",15:"内蒙古",21:"辽宁",22:"吉林",23:"黑龙江 ",31:"上海",32:"江苏",33:"浙江",34:"安徽",35:"福建",36:"江西",37:"山东",41:"河南",42:"湖北 ",43:"湖南",44:"广东",45:"广西",46:"海南",50:"重庆",51:"四川",52:"贵州",53:"云南",54:"西藏 ",61:"陕西",62:"甘肃",63:"青海",64:"宁夏",65:"新疆",71:"台湾",81:"香港",82:"澳门",91:"国外"};
            var birthday = value.substr(6, 4) + '/' + Number(value.substr(10, 2)) + '/' + Number(value.substr(12, 2));
            var d = new Date(birthday);
            var newBirthday = d.getFullYear() + '/' + Number(d.getMonth() + 1) + '/' + Number(d.getDate());
            var currentTime = new Date().getTime();
            var time = d.getTime();
            var arrInt = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2];
            var arrCh = ['1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2'];
            var sum = 0, i, residue;
            for(i=0; i<17; i++) {
                sum += value.substr(i, 1) * arrInt[i];
            }
            residue = arrCh[sum % 11];
            if(!/^\d{17}(\d|x)$/i.test(value)){
                this.$message.error('请输入正确的身份证号码');
                return false
            }else if(city[value.substr(0,2)] === undefined) {
                this.$message.error('身份证地区不合法');
                return false
            }else if(time >= currentTime || birthday !== newBirthday){
                this.$message.error('身份证生日不合法');
                return false
            }else if (residue !== value.substr(17, 1)) {
                this.$message.error('非法身份证');
                return false
            }else{
                callback()
            }
            return city[value.substr(0,2)]+","+birthday+","+(value.substr(16,1)%2?" 男":"女")            
        }
        var phoneNumber = (rule, value, callback) => {
             if (!/^1[345678][0-9]{9}$/.test(value)) {
                 console.log(value)
                return callback(new Error('手机号码不能为空'));
            }else{
                callback()
            }
        }
        return{
            personId:"",
            value1:"",
            //租客信息
            landlordsInfo:"",
            dataForm:{
                name:"",
                sex:"",
                nation:"",
                birthday:"",
                identityCard:"",
                phone:"",
                id:"",
            },
            addDataForm:{
                name:"",
                sex:"",
                nation:"",
                birthday:"",
                identityCard:"",
                phone:"",
                id:"",
            },
            nation:[],//民族列表
            landlordsInfoNation:"",//租客民族
            landlordsInfoNationValue:"",//租客民族Value
            landlordsInfoSexValue:"",//租客性别Value,
            landlordsInfoBirthday:"",//租客生日
            landlordApartments:[],//租客房屋列表
            renterApartments:[],//租客房屋列表
            dialogFormVisible:false,//编辑模态框
            dataListLoading:false,//租客房屋列表loding
            dataFormRule:{
                name:[
                    { required: true, message: '姓名不能为空', trigger: 'blur' }
                ],
                sex:[
                    { required: true, message: '性别不能为空', trigger: 'blur' }
                ],
                nation:[
                    { required: true, message: '民族不能为空', trigger: 'blur' }
                ],
                birthday:[
                    { required: true, message: '出生日期不能为空', trigger: 'blur' }
                ],
                identityCard:[
                    {validator: identityCard,required: true, message: '请填写正确的身份证号码', trigger: 'blur'}
                ],
                phone:[
                    {validator:phoneNumber,required: true, message: '请填写正确的手机号码', trigger: 'blur'}
                ]
            }
        }
    },
    activated (){
        this.personId = Number(this.$cookie.get('id'));//取cookie
        this.landlordsData();
        this.nationSelect();
    },
    methods:{
        // isDisabled(label){
        //     if(parseInt(this.landlordsInfoSexValue) == parseInt(label)){
        //         console.log(this.landlordsInfoSexValue)
        //         console.log(label)
                
        //         return false
        //     }
        //     return !this.landlordsInfoSexValue.disabled(label)
        //    // return !this.dataForm.sex.includes(label)
        // },
        //获取民族下拉列表
        nationSelect(){
             this.$http({
                url: this.$http.adornUrl(`/pub/dicdetail`),
                method: "post",
                data: this.$http.adornData({"value": 1014})
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
        landlordsData(data){
            this.dataListLoading = true;
            this.$http({
                url: this.$http.adornUrl(`/person/querybyid`),
                method: "post",
                data: this.$http.adornData({personId:this.personId})
            })
            .then(data=> {
                this.dataListLoading = false;
                if(data.data.resultCode==1000){
                    this.landlordsInfo = data.data.resultData.person;
                    this.dataForm.name = data.data.resultData.person.name;
                    this.dataForm.sex = data.data.resultData.person.sex.value;
                    this.dataForm.nation = data.data.resultData.person.nation.title;
                    this.dataForm.birthday = data.data.resultData.person.birthday;
                    this.dataForm.identityCard = data.data.resultData.person.identityCard;
                    this.dataForm.phone = data.data.resultData.person.phone;
                    this.dataForm.id = data.data.resultData.person.id;

                    this.landlordsInfoSexValue = data.data.resultData.person.sex.value;
                    this.landlordsInfoNation = data.data.resultData.person.nation.title;
                    this.landlordsInfoNationValue = data.data.resultData.person.nation.value;
                    this.landlordsInfoBirthday = data.data.resultData.person.birthday.substr(0, 4)+ ' 年'+ " " +data.data.resultData.person.birthday.substr(4, 2)+ ' 月'+ " " +data.data.resultData.person.birthday.substr(6, 2)+ ' 日';
                    // 房东房屋列表
                    this.landlordApartments = data.data.resultData.landlordApartments;
                    // 租客房屋列表
                    this.renterApartments = data.data.resultData.renterApartments;
                }    
            })
            .catch(function(err) {
                console.log("失败");
            });
        },
        //民族列表改变把value值 赋给landlordsInfoNationValue
        changeNation(){
            this.landlordsInfoNationValue = this.dataForm.nation ;
        },
        //租客编辑提交
        landlordsEditSure(dataForm){
            this.$refs[dataForm].validate((valid) => {
                if(valid){
                    var data = {
                        "type":1,
                        "name":this.dataForm.name,
                        "sex":parseInt(this.dataForm.sex),
                        "birthday":this.dataForm.birthday,
                        "nation":parseInt(this.landlordsInfoNationValue),
                        "identityCard":this.dataForm.identityCard,
                        "phone":this.dataForm.phone,
                        "id":parseInt(this.dataForm.id)
                    }
                    this.$http({
                        url: this.$http.adornUrl(`/person/edit`),
                        method: "post",
                        data: this.$http.adornData(data)
                    })
                    .then(data=>{
                        if(data.data.resultCode==1000){
                            this.$message({
                                message: '编辑成功',
                                type: 'success',
                                duration: 1500,
                                onClose: () => {
                                    this.dialogFormVisible = false;
                                    this.landlordsData();
                                    this.$refs['dataForm'].resetFields();
                                }
                            }) 
                        }
                        if(data.data.code === 500){
                            this.$message.error(data.data.msg);
                        }     
                    })
                    .catch(function(err) {
                        console.log("失败");
                    });
                }
            })
        },
    }
}
</script>
<style scoped>
    .landlordsInfo_icon{
        width: 20px;
        height: 20px;
        vertical-align: text-bottom;
        margin-right: 10px;
    }
    .landlordsInfo_sapn{
        margin-left: 20px;
        cursor: pointer;
    }
    .landlordsInfo_data .el-form-item{
        margin-bottom: 5px;
    }
    .el-input{
        width: 220px;
    }
    .el-select{
        width: 220px;
    }
    .el-dialog .el-form-item{
        display: inline-block;
    }
</style>
