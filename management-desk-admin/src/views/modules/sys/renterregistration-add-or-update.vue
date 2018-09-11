<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()">
      <el-form-item label="姓名" prop="name">
        <el-input v-model="dataForm.name" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="籍贯" prop="nativePlace">
        <el-input v-model="dataForm.nativePlace" placeholder="籍贯"></el-input>
      </el-form-item>
      <el-form-item label="民族" prop="nation">
        <el-input v-model="dataForm.nation" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="身份证号" prop="idCard">
        <el-input v-model="dataForm.idCard" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="婚姻状态.0-未婚，1-已婚，2-离异" prop="maritalStatus">
        <el-input v-model="dataForm.maritalStatus" placeholder="婚姻状态.0-未婚，1-已婚，2-离异"></el-input>
      </el-form-item>
      <el-form-item label="电话" prop="phone">
        <el-input v-model="dataForm.phone" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="公司" prop="compay">
        <el-input v-model="dataForm.compay" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="所属社区" prop="communityCode">
        <el-input v-model="dataForm.communityCode" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="住户Id" prop="residentialId">
        <el-input v-model="dataForm.residentialId" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="地址" prop="address">
        <el-input v-model="dataForm.address" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="身份证正面照" prop="idCardFrontPic">
        <el-input v-model="dataForm.idCardFrontPic" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="身份证反面照" prop="idCardBackPic">
        <el-input v-model="dataForm.idCardBackPic" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="免冠照" prop="avatarUrl">
        <el-input v-model="dataForm.avatarUrl" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="状态" prop="status">
        <el-input v-model="dataForm.status" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="创建时间" prop="createTime">
        <el-input v-model="dataForm.createTime" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="修改时间" prop="modifyTime">
        <el-input v-model="dataForm.modifyTime" placeholder=""></el-input>
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
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          name: '',
          nativePlace: '',
          nation: '',
          idCard: '',
          maritalStatus: '',
          phone: '',
          compay: '',
          communityCode: '',
          residentialId: '',
          address: '',
          idCardFrontPic: '',
          idCardBackPic: '',
          avatarUrl: '',
          status: '',
          createTime: '',
          modifyTime: ''
        },
        dataRule: {
          name: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          nativePlace: [
            { required: true, message: '籍贯不能为空', trigger: 'blur' }
          ],
          nation: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          idCard: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          maritalStatus: [
            { required: true, message: '婚姻状态.0-未婚，1-已婚，2-离异不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          compay: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          communityCode: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          residentialId: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          idCardFrontPic: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          idCardBackPic: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          avatarUrl: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          modifyTime: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/sys/renterregistration/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.renterRegistration.name
                this.dataForm.nativePlace = data.renterRegistration.nativePlace
                this.dataForm.nation = data.renterRegistration.nation
                this.dataForm.idCard = data.renterRegistration.idCard
                this.dataForm.maritalStatus = data.renterRegistration.maritalStatus
                this.dataForm.phone = data.renterRegistration.phone
                this.dataForm.compay = data.renterRegistration.compay
                this.dataForm.communityCode = data.renterRegistration.communityCode
                this.dataForm.residentialId = data.renterRegistration.residentialId
                this.dataForm.address = data.renterRegistration.address
                this.dataForm.idCardFrontPic = data.renterRegistration.idCardFrontPic
                this.dataForm.idCardBackPic = data.renterRegistration.idCardBackPic
                this.dataForm.avatarUrl = data.renterRegistration.avatarUrl
                this.dataForm.status = data.renterRegistration.status
                this.dataForm.createTime = data.renterRegistration.createTime
                this.dataForm.modifyTime = data.renterRegistration.modifyTime
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/sys/renterregistration/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'nativePlace': this.dataForm.nativePlace,
                'nation': this.dataForm.nation,
                'idCard': this.dataForm.idCard,
                'maritalStatus': this.dataForm.maritalStatus,
                'phone': this.dataForm.phone,
                'compay': this.dataForm.compay,
                'communityCode': this.dataForm.communityCode,
                'residentialId': this.dataForm.residentialId,
                'address': this.dataForm.address,
                'idCardFrontPic': this.dataForm.idCardFrontPic,
                'idCardBackPic': this.dataForm.idCardBackPic,
                'avatarUrl': this.dataForm.avatarUrl,
                'status': this.dataForm.status,
                'createTime': this.dataForm.createTime,
                'modifyTime': this.dataForm.modifyTime
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
