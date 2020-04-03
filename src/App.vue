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
          <el-option label="请选择" value="choose" disabled="disabled"></el-option>
          <el-option label="身份证" value="ID"></el-option>
          <el-option label="军官证" value="officers"></el-option>
          <el-option label="护照" value="passport"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="phone">
        <label for="">联系手机</label>
        <span class="tips">*</span>
        <el-input v-model="ruleForm.phone"></el-input>
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
         <label for="">家庭住址</label>
         <span class="tips">*</span>
          <div>
           <mt-picker :slots="myAddressSlots" @change="onMyAddressChange"></mt-picker>
            <p>地址：{{myAddressProvince}} {{myAddressCity}} {{myAddresscounty}}</p>
          </div>
      </el-form-item>
      <el-form-item prop="addressDetail">
        <label for="">详细地址</label>
        <span class="tips">*</span>
        <el-input v-model="ruleForm.addressDetail"></el-input>
      </el-form-item>
       <el-form-item prop="addressDetailInBJ" >
        <label>在京详细地址</label>
        <el-input v-model="ruleForm.addressDetailInBJ" placeholder="如无可不填写"></el-input>
      </el-form-item>
      <el-form-item prop="agree">
        <label for="">同意提交</label>
        <span class="tips">*</span>
        <el-radio-group v-model="this.ruleForm.radio">
          <el-radio @click.native.prevent="clickitem(1)" :label="1">本人保证以上填写信息真实无隐瞒</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-button type="success" @click="getDate" :style="{background: color}">提交</el-button>
    </el-form>
  </el-main>
</el-container>
</template>

<script>
import axios from 'axios';
import { Picker } from 'mint-ui';
 import myaddress from './json/city.json'
export default {
  name: 'Table',
  components: {
  'mt-picker': Picker
  },
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
          radio: 0,
        },
    color: '#1ab394',
    phoneerrmsg: '',
    regionVisible: false,
    region: '',
    myAddressSlots: [
      {
      flex: 1,
      defaultIndex: 1, 
      values: Object.keys(myaddress), //省份数组
      className: 'slot1',
      textAlign: 'center'
      }, {
      divider: true,
      content: '-',
      className: 'slot2'
      }, {
      flex: 1,
      values: [],
      className: 'slot3',
      textAlign: 'center'
      },
      {
      divider: true,
      content: '-',
      className: 'slot4'
      },
      {
      flex: 1,
      values: [],
      className: 'slot5',
      textAlign: 'center'
      }
      ],
      myAddressProvince:'省',
      myAddressCity:'市',
      myAddresscounty:'区/县'
}},
 mounted(){
    this.$nextTick(() => { //vue里面全部加载好了再执行的函数 （类似于setTimeout）
    this.myAddressSlots[0].defaultIndex = 0 
    });
 },
  methods: {
    clickitem (e) {
      e === this.ruleForm.radio ? this.ruleForm.radio = '' : this.ruleForm.radio = e
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
      }else if(this.ruleForm.phone!=""){
          if(!(/^1[3456789]\d{9}$/.test(this.ruleForm.phone))){ 
            this.$message({
              showClose: true,
              message: '手机号码有误，请重填!',
              type: 'warning',
              offset: '300px'
            });
          return
        } 
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
    },
     onMyAddressChange(picker, values) {
        if(myaddress[values[0]]){ //这个判断类似于v-if的效果（可以不加，但是vue会报错，很不爽）
        picker.setSlotValues(1,Object.keys(myaddress[values[0]])); // Object.keys()会返回一个数组，当前省的数组
        picker.setSlotValues(2,myaddress[values[0]][values[1]]); // 区/县数据就是一个数组
        this.myAddressProvince = values[0];
        this.myAddressCity = values[1];
        this.myAddresscounty = values[2];
        }
    },
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
      .el-form-item:nth-child(2){
        .el-form-item__content{
          .el-select{
            width: 100% !important;
            .el-input{
              width: 93% !important;
          }
        }
      }
      }
      el-form-item:nth-child(4){
        .el-form-item__content{
          div{
            .control{
              input{
                width: 9rem;
                padding: 0.32rem;
                margin: 0.08rem  auto;
                border-radius: 0.08rem;
                border: 0.0267rem solid #e1e1e1;
                background: #ffff;
                font-size: 0.4267rem ;
              }
            }
            .mint-popup{
               width: 10rem;
            }
          }
        }
      }
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
            width:30% !important;
            .el-input{
              width: 100%;
            }
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
          .distpicker-address-wrapper{
            width: 93%;
            select{
              padding: 0;
              width: 32%;
            }
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
