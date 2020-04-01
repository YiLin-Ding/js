<template>
<el-container>
  <el-header>
    <i class="el-icon-edit"></i>
    <span>入院问诊信息登记表</span>
  </el-header>
  <el-main>
    <el-form :model="ruleForm"  ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-form-item prop="name">
        <label for="">姓名</label>
        <span class="tips">*</span>
        <el-input v-model="ruleForm.name"></el-input>
      </el-form-item>
      <el-form-item prop="type">
        <label for="">证件类型</label>
        <span class="tips">*</span>
        <el-select v-model="ruleForm.type" placeholder="请选择">
          <el-option label="请选择" value="choose"></el-option>
          <el-option label="身份证" value="ID"></el-option>
          <el-option label="军官证" value="officers"></el-option>
          <el-option label="护照" value="passport"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="phone">
        <label for="">联系手机</label>
        <span class="tips">*</span>
        <el-input v-model="ruleForm.phone" @input="inputPhone"></el-input>
      </el-form-item>
      <el-form-item prop="
purpose">
        <label for="">是否专程来京就医</label>
        <span class="tips">*</span>
        <el-radio-group v-model="ruleForm.purpose">
          <el-radio label="是" value="1"></el-radio>
          <el-radio label="否" value="2"></el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item>
         <label for="">选择地区</label>
         <span class="tips">*</span>
        <el-select size="small" style="width: 100px"
          v-model="ruleForm.selectProv"
          placeholder="请选择省份"
          v-on:change="getProv($event)">
          <el-option
            v-for="item in provs"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-select size="small" style="width: 100px"
          v-if="ruleForm.selectProv!=''"
          v-model="ruleForm.selectCity"
          placeholder="请选择城市"
          v-on:change="getCity($event)">
          <el-option
            v-for="item in citys"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-select size="small" style="width: 100px"
          v-if="ruleForm.selectCity!=''"
          v-model="ruleForm.selectArea"
          placeholder="请选择区"
          v-on:change="getArea($event)">
          <el-option
            v-for="item in area"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="addressDetail">
        <label for="">详细地址</label>
        <span class="tips">*</span>
        <el-input v-model="ruleForm.addressDetail"></el-input>
      </el-form-item>
       <el-form-item prop="addressDetailInBJ" placeholder="如无可不填写">
        <label placeholder="入无可不填写">在京详细地址</label>
        <el-input v-model="ruleForm.addressDetailInBJ"></el-input>
      </el-form-item>
      <el-form-item prop="agree">
        <label for="">同意提交</label>
        <span class="tips">*</span>
        <el-radio v-model="ruleForm.radio" label="1">本人保证以上填写信息真实无隐瞒</el-radio>
      </el-form-item>
      <el-button type="success" @click="getDate">提交</el-button>
    </el-form>
  </el-main>
</el-container>
</template>

<script>
export default {
  name: 'Table',
  data () {
    return {
      ruleForm: {
          name: '',
          type: '',
          phone: '',
          purpose: '',
          address: '',
          addressDetail: '',
          agree: '',
          radio: '',
          selectProv: '',
          selectCity: '',
          selectArea: ''
        },
     provs:[{label:"北京市",value:"北京市"},
          {label:"天津市",value:"天津市"},
          {label:"河北省",value:"河北省"},
          {label:"山西省",value:"山西省"},
          {label:"内蒙古自治区",value:"内蒙古自治区"},
          {label:"辽宁省",value:"辽宁省"},
          {label:"吉林省",value:"吉林省"}],
    citys: [],
    area: [],
    phoneerrmsg: ''
}},
  methods: {
    getProv: function (prov) {
    let tempCity=[];
    this.citys=[];
    this.ruleForm.selectCity='';
    let allCity=[
      {
      prov: "北京市",
      label: "北京市"
    }, {
      prov: "天津市",
      label: "天津市"
    }, {
      prov: "河北省",
      label: "石家庄市"
    }, {
      prov: "河北省",
      label: "唐山市"
    }]
    for (var val of allCity){
    if (prov == val.prov){
      tempCity.push({label: val.label, value: val.label})
      }
    }
    this.citys = tempCity;
  }, 
   inputPhone(e){
    console.log(e)
      this.phoneerrmsg = '';   //验证输入的提示信息
      let filtered = e.replace(/^0|[^\d]/g, '');  
      if(this.ruleForm.phone != filtered){
          alert('error')
      }
    },
    getCode(){
    //获取手机验证码
    let reg = /^1[3-9][0-9]{9}$/;  //以1开头第二位数字为3-9 的11位数字
    if(this.phone.length == 0){
        this.phoneerrmsg = '请输入手机号';
        return;
    }else if(!reg.test(this.phone)){
        this.phoneerrmsg = '请输入正确的手机号';
        return;
    }else{
        this.phoneerrmsg = '';
    }
    },
    getCity (city) {
      let tempArea=[];
      this.area=[];
       this.ruleForm.selectArea='';
      console.log('getCity:' + city);
      let allArea = [
        {
          city: "北京市",
          label: "昌平区",
          value: "昌平区",
        },{
          city: "北京市",
          label: "五环区",
          value: "五环区",
        },{
          city: "厦门市",
          label: "新林县",
          value: "新林县",
        },{
          city: "厦门市",
          label: "新罗区",
          value: "新罗区",
        },{
          city: "厦门市",
          label: "湖里",
          value: "湖里",
        }]
        for (var val of allArea){
          if (city == val.city){
            tempArea.push({label: val.label, value: val.label})
          }
        }
        this.area = tempArea;
    },
    getArea(area){
      console.log('getArea:' + area);
    },
    getDate: function(){
      if(this.ruleForm.name==""){
        this.$message({
          showClose: true,
          message: '用户名不能为空!',
          type: 'warning',
          offset: '300px'
        });
        return
      }
      else if(this.ruleForm.type==""){
        this.$message({
          showClose: true,
          message: '类型不能为空!',
          type: 'warning',
          offset: '300px'
        });
        return
      }else if(this.ruleForm.phone==""){
        this.$message({
          showClose: true,
          message: '手记号码不能为空!',
          type: 'warning',
          offset: '300px'
        });
        return
      }else if(this.ruleForm.purpose==""){
        this.$message({
          showClose: true,
          message: '是否来京就医不能为空!',
          type: 'warning',
          offset: '300px'
        });
        return
      }else if(this.ruleForm.selectProv==""||this.ruleForm.selectCity==""||this.ruleForm.selectArea==""){
        this.$message({
          showClose: true,
          message: '选择地区不能为空!',
          type: 'warning',
          offset: '300px'
        });
        return
      }else if(this.ruleForm.addressDetail==""){
        this.$message({
          showClose: true,
          message: '详细地址不能为空!',
          type: 'warning',
          offset: '300px'
        });
        return
      }else if(this.ruleForm.radio==""){
        this.$message({
          showClose: true,
          message: '同意提交不能为空!',
          type: 'warning',
          offset: '300px'
        });
        return
      }else{
       console.log(this.ruleForm)
      }
    }
  }
}
</script>

<style lang="less">
@import "css/reset.css";
  .el-container{
  .el-header{
    background: #eee;
    color: #666;
    position:fixed;
    z-index:4;
    width:100%;
    i{
      font-size: 18px;
      margin-right: 10px;
    }
    span{
      font-size: 18px;
      line-height: 60px;
    }
  }
  .el-main{
    margin-top:60px;
    padding: 0;
    .el-form{
      .el-form-item{
        .el-form-item__label{
          width: 100% !important;
        }
        .tips{
          color: red;
        }
        .el-form-item__content{
          margin-left:30px !important;
          .el-input{
            width: 93%;
          }
          .el-select{
            width:100% !important;
          }
          .el-radio-group{
            width: 100%;
            .el-radio{
              width: auto;
            }
          }
          .el-radio{
            width:100%;
          }
        }
        label{
          text-align: left;

        }
        .el-radio-group {
          span{
            color: black;
          }
        }
        .radio{
          .el-radio__label{
            color: black;
          }
        }
      }
    }
    button{
      width:90%;
      margin:0 5% 20px;
    }
}
}
</style>
