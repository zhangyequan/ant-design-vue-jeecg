<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
          <a-col :span="24">
            <a-form-model-item label="被访对象姓名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitedname">
              <a-input v-model="model.visitedname" placeholder="请输入被访对象姓名"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="被访对象手机号码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitedpnum">
              <a-input v-model="model.visitedpnum" placeholder="请输入被访对象手机号码"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="来访事由" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitedevent">
              <a-input v-model="model.visitedevent" placeholder="请输入来访事由"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="来访时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visittime">
              <j-date placeholder="请选择来访时间" v-model="model.visittime"  style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="离开时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="lefttime">
              <j-date placeholder="请选择离开时间" v-model="model.lefttime"  style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="来访者姓名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitorname">
              <a-input v-model="model.visitorname" placeholder="请输入来访者姓名"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="来访者手机号码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitorpnum">
              <a-input v-model="model.visitorpnum" placeholder="请输入来访者手机号码"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="身份证号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="identitynum">
              <a-input v-model="model.identitynum" placeholder="请输入身份证号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="车牌号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="platenum">
              <a-input v-model="model.platenum" placeholder="请输入车牌号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="图片路径" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="imgurl">
              <a-input v-model="model.imgurl" placeholder="请输入图片路径"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="状态"" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="status">
              <a-input v-model="model.status" placeholder="请输入状态"  ></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </j-form-container>
  </a-spin>
</template>

<script>

  import { httpAction, getAction } from '@/api/manage'
  import { validateDuplicateValue } from '@/utils/util'

  export default {
    name: 'VisitinfoForm',
    components: {
    },
    props: {
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    data () {
      return {
        model:{
         },
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        validatorRules: {
           visitedname: [
              { required: true, message: '请输入被访对象姓名!'},
           ],
           visitedpnum: [
              { required: true, message: '请输入被访对象手机号码!'},
           ],
           visitedevent: [
              { required: true, message: '请输入来访事由!'},
           ],
           visittime: [
              { required: true, message: '请输入来访时间!'},
           ],
           lefttime: [
              { required: true, message: '请输入离开时间!'},
           ],
           visitorname: [
              { required: true, message: '请输入来访者姓名!'},
           ],
           visitorpnum: [
              { required: true, message: '请输入来访者手机号码!'},
           ],
           identitynum: [
              { required: true, message: '请输入身份证号!'},
           ],
           platenum: [
              { required: true, message: '请输入车牌号!'},
           ],
           imgurl: [
              { required: true, message: '请输入图片路径!'},
           ],
           status: [
              { required: true, message: '请输入状态!'},
           ],
        },
        url: {
          add: "/visit/visitinfo/add",
          edit: "/visit/visitinfo/edit",
          queryById: "/visit/visitinfo/queryById"
        }
      }
    },
    computed: {
      formDisabled(){
        return this.disabled
      },
    },
    created () {
       //备份model原始值
      this.modelDefault = JSON.parse(JSON.stringify(this.model));
    },
    methods: {
      add () {
        this.edit(this.modelDefault);
      },
      edit (record) {
        this.model = Object.assign({}, record);
        this.visible = true;
      },
      submitForm () {
        const that = this;
        // 触发表单验证
        this.$refs.form.validate(valid => {
          if (valid) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            httpAction(httpurl,this.model,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
            })
          }
         
        })
      },
    }
  }
</script>