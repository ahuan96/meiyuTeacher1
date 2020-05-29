<template>
  <el-dialog width="480px" title="特长认证申请" oncontextmenu="return false" onselectstart="return false"
    :close-on-click-modal="false"
    :visible.sync="isShowing"
    v-if="isShowing">
    <el-form ref="elform" label-width="90px">
      <el-form-item label="认证内容">{{params.title}}</el-form-item>

      <el-form-item label="证书级别">{{params.gradeName}}</el-form-item>

      <el-form-item label="技能描述">{{params.content}}</el-form-item>

      <el-form-item label="上传档案">{{params.imgs}}</el-form-item>
    </el-form>
  </el-dialog>
</template>

<script>
import mixmodal from '../mixin/modal'

export default {
  mixins: [mixmodal],
  components: {
  },
  props: [
    'formData'
  ],
  data () {
    return {
      params: {} // 参数
    }
  },
  watch: {
  },
  computed: {
  },
  methods: {
    /**
     * [initial 初始化组件]
     * @return {[]} []
     */
    initial () {
      Object.assign(this.params, this.formData)

      this.askDatas(() => {
        this.toggleShow()
      })
    },

    /**
     * [askDatas 请求数据]
     * @param  {[Function]} cbk [回调]
     * @return {[]} []
     */
    askDatas (cbk) {
      let $rt = this.$get('student_certificate/info/', this.params)
      $rt.then((rt) => {
        this.params = rt.data.row

        if (this.params.grade === '1') {
          this.params.gradeName = '国家级'
        } else if (this.params.grade === '2') {
          this.params.gradeName = '省级'
        } else if (this.params.grade === '3') {
          this.params.gradeName = '市级'
        } else if (this.params.grade === '4') {
          this.params.gradeName = '区县级'
        } else {
          this.params.gradeName = '其他'
        }

        cbk()
      }).catch((rt) => {
      })
    }
  },
  created () {
  },
  mounted () {
  }
}
</script>
