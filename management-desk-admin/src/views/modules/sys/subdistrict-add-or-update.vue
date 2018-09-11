<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="所属区" prop="cityCode">
        <!-- <el-input v-model="dataForm.cityCode" placeholder="所属区编码"></el-input> -->
        <el-select v-model="dataForm.cityCode" clearable placeholder="请选择区" style="width:100%">
          <el-option v-for="item in area" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="编码" prop="code">
        <el-input v-model="dataForm.code" placeholder="格式：城市代码6位 + 3位按顺序编码"></el-input>
      </el-form-item>
      <el-form-item label="名称" prop="name">
        <el-input v-model="dataForm.name" placeholder="名称"></el-input>
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
          code: '',
          name: '',
          cityCode: '',
          createTime: '',
          modifyTime: ''
        },
        area: [
          { value: '330102', label: "上城区" },
          { value: '330103', label: "下城区" },
          { value: '330104', label: "江干区" },
          { value: '330105', label: "拱墅区" },
          { value: '330106', label: "西湖区" },
          { value: '330108', label: "滨江区" },
          { value: '330109', label: "萧山区" },
          { value: '330110', label: "余杭区" },
          { value: '330111', label: "富阳区" },
          { value: '330112', label: "临安区" },
          { value: '330122', label: "桐庐县" },
          { value: '330127', label: "淳安县" },
          { value: '330182', label: "建德市" }
        ],
        dataRule: {
          code: [
            { required: true, message: '代码。格式：城市代码6位 + 6位按顺序编码不能为空', trigger: 'blur' }
          ],
          name: [
            { required: true, message: '名称不能为空', trigger: 'blur' }
          ],
          cityCode: [
            { required: true, message: '所属城市代码不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/sys/subdistrict/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.code = data.subdistrict.code
                this.dataForm.name = data.subdistrict.name
                this.dataForm.cityCode = data.subdistrict.cityCode
                this.dataForm.createTime = data.subdistrict.createTime
                this.dataForm.modifyTime = data.subdistrict.modifyTime
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
              url: this.$http.adornUrl(`/sys/subdistrict/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'code': this.dataForm.code,
                'name': this.dataForm.name,
                'cityCode': this.dataForm.cityCode,
                'createTime': this.dataForm.id === 0 ? new Date().getTime() : null,
                'modifyTime': new Date().getTime()
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
