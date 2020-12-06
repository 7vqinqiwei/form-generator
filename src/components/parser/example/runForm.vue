<template>
  <div class="test-form">
    <parser :form-conf="formConf" @submit="sumbitForm" />
  </div>
</template>

<script>
import Parser from '../Parser'

// 若parser是通过安装npm方式集成到项目中的，使用此行引入
// import Parser from 'form-gen-parser'

export default {
  components: {
    Parser
  },
  props: {},
  data() {
    return {
      formConf: {}
    }
  },
  computed: {},
  watch: {},
  created() {
    // 异步请求表单定义json数据或者高级模式HTML/script/css所有的定义
    this.$axios.get('http://baidu.com').then(resp => {
      this.formConf = {}
    }).then(err => {
      console.log(err)
    })
  },
  mounted() {
    setTimeout(() => {
      // 表单数据回填，模拟异步请求场景
      // 请求回来的表单数据
      // 回填数据
      this.fillFormData(this.formConf, {})
    }, 2000)
  },
  methods: {
    fillFormData(form, data) {
      if (!form || !form.fields) return
      if (!data) return
      form.fields.forEach(item => {
        const val = data[item.__vModel__]
        if (val) {
          item.__config__.defaultValue = val
        }
      })
    },
    sumbitForm(data) {
      console.log('sumbitForm1提交数据：', data)
    }
  }
}
</script>

<style lang="scss" scoped>
.test-form {
  margin: 15px auto;
  width: 800px;
  padding: 15px;
}
</style>
