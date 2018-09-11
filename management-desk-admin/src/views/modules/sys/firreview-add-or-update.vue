<template>
  <el-dialog
    :title="详情"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="姓名" prop="landlordName">
        <el-input v-model="dataForm.landlordName" placeholder="" disabled></el-input>
      </el-form-item>
      <el-form-item label="电话" prop="landlordPhone">
        <el-input v-model="dataForm.landlordPhone" placeholder="" disabled></el-input>
      </el-form-item>
      <el-form-item label="所属社区" prop="communityCode">
        <el-input v-model="dataForm.communityCode" placeholder="" disabled></el-input>
      </el-form-item>
      <el-form-item label="所属小区" prop="residentialId">
        <el-input v-model="dataForm.residentialId" placeholder="" disabled></el-input>
      </el-form-item>
      <el-form-item label="地址" prop="address">
        <el-input v-model="dataForm.address" placeholder="" disabled></el-input>
      </el-form-item>
      <el-form-item label="创建人" prop="createSysUid">
        <el-input v-model="dataForm.createSysUid" placeholder="" disabled></el-input>
      </el-form-item>
      <el-form-item label="审核人" prop="auditSysUid">
        <el-input v-model="dataForm.auditSysUid" placeholder="" disabled></el-input>
      </el-form-item>
      <el-form-item label="审核时间" prop="auditTime">
        <el-input v-model="dataForm.auditTime" placeholder="" disabled></el-input>
      </el-form-item>
      <el-form-item label="状态" prop="status">
        <el-input v-model="dataForm.status" placeholder="0-审核中，1-审核成功，2-审核失败" disabled></el-input>
      </el-form-item>
      <el-form-item label="结果" prop="failMessage">
        <el-input v-model="dataForm.failMessage" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="创建时间" prop="createTime">
        <el-input v-model="dataForm.createTime" placeholder="" disabled></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button><!-- 
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button> -->
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
          landlordName: '',
          landlordPhone: '',
          communityCode: '',
          residentialId: '',
          address: '',
          createSysUid: '',
          auditSysUid: '',
          auditTime: '',
          status: '',
          failMessage: '',
          createTime: '',
          modifyTime: ''
        },
        dataRule: {
          landlordName: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          landlordPhone: [
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
          createSysUid: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          auditSysUid: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          auditTime: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '0-审核中，1-审核成功，2-审核失败不能为空', trigger: 'blur' }
          ],
          failMessage: [
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
              url: this.$http.adornUrl(`/sys/firreview/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.landlordName = data.firReview.landlordName
                this.dataForm.landlordPhone = data.firReview.landlordPhone
                this.dataForm.communityCode = data.firReview.communityCode
                this.dataForm.residentialId = data.firReview.residentialId
                this.dataForm.address = data.firReview.address
                this.dataForm.createSysUid = data.firReview.createSysUid
                this.dataForm.auditSysUid = data.firReview.auditSysUid
                this.dataForm.auditTime = data.firReview.auditTime
                this.dataForm.status = data.firReview.status
                this.dataForm.failMessage = data.firReview.failMessage
                this.dataForm.createTime = data.firReview.createTime
                this.dataForm.modifyTime = data.firReview.modifyTime
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
              url: this.$http.adornUrl(`/sys/firreview/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'landlordName': this.dataForm.landlordName,
                'landlordPhone': this.dataForm.landlordPhone,
                'communityCode': this.dataForm.communityCode,
                'residentialId': this.dataForm.residentialId,
                'address': this.dataForm.address,
                'createSysUid': this.dataForm.createSysUid,
                'auditSysUid': this.dataForm.auditSysUid,
                'auditTime': this.dataForm.auditTime,
                'status': this.dataForm.status,
                'failMessage': this.dataForm.failMessage,
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
