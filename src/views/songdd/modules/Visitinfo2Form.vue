<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
          <a-col :span="24">
            <a-form-model-item label="visitedname" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitedname">
              <a-input v-model="model.visitedname" placeholder="请输入visitedname"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="visitednum" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitednum">
              <a-input v-model="model.visitednum" placeholder="请输入visitednum"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="visitedevent" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitedevent">
              <a-input v-model="model.visitedevent" placeholder="请输入visitedevent"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="visittime" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visittime">
              <j-date placeholder="请选择visittime" v-model="model.visittime"  style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="lefttime" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="lefttime">
              <j-date placeholder="请选择lefttime" v-model="model.lefttime"  style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="visitname" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitname">
              <a-input v-model="model.visitname" placeholder="请输入visitname"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="visitnum" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="visitnum">
              <a-input v-model="model.visitnum" placeholder="请输入visitnum"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="identitynum" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="identitynum">
              <a-input v-model="model.identitynum" placeholder="请输入identitynum"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="carnum" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="carnum">
              <a-input v-model="model.carnum" placeholder="请输入carnum"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="imgurl" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="imgurl">
              <a-input v-model="model.imgurl" placeholder="请输入imgurl"  ></a-input>
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
    name: 'Visitinfo2Form',
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
              { required: true, message: '请输入visitedname!'},
           ],
           visitednum: [
              { required: true, message: '请输入visitednum!'},
           ],
           visitedevent: [
              { required: true, message: '请输入visitedevent!'},
           ],
           visittime: [
              { required: true, message: '请输入visittime!'},
           ],
           lefttime: [
              { required: true, message: '请输入lefttime!'},
           ],
           visitname: [
              { required: true, message: '请输入visitname!'},
           ],
           visitnum: [
              { required: true, message: '请输入visitnum!'},
           ],
           identitynum: [
              { required: true, message: '请输入identitynum!'},
           ],
           carnum: [
              { required: true, message: '请输入carnum!'},
           ],
           imgurl: [
              { required: true, message: '请输入imgurl!'},
           ],
        },
        url: {
          add: "/songdd/visitinfo2/add",
          edit: "/songdd/visitinfo2/edit",
          queryById: "/songdd/visitinfo2/queryById"
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