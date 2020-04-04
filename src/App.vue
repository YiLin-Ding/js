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
         <input type="text" placeholder="请选择" v-model="ruleForm.type" maxlength="80" readonly="readonly" @click="showIdPicker" class="setInput"/>
          <mt-popup v-model="idVisible" position="bottom">
            <div class="cateContainer" @click.stop>
                <div class="operateBtn">
                  <div class="confirmBtn btn" @click="handleFinishId">完成</div>
                </div> 
                <mt-picker value-key="name" :slots="myId" @change="onIdChange" :showToolbar="true"></mt-picker>
            </div>
          </mt-popup>
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
         <input type="text" placeholder="请选择" v-model="ruleForm.address" maxlength="80" readonly="readonly" @click="showAddressPicker" class="setInput"/>
         <mt-popup v-model="regionVisible" position="bottom">
         <div class="cateContainer" @click.stop>
            <div class="operateBtn">
              <div class="cancelBtn btn" @click="handleCloseAddress">取消</div>
              <div class="confirmBtn btn" @click="handleFinishAddress">完成</div>
            </div>
            <mt-picker class="addressPicker" :slots="myAddressSlots" @change="onAddressChange" :showToolbar="true"></mt-picker>
         </div>
         </mt-popup>   
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
import addressJson from './json/city.json';
import $ from 'jquery';
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
          radio: 0
        },
    color: '#1ab394',
    phoneerrmsg: '',
    regionVisible: false,
    region: '',
    myAddressSlots: [
      {
        flex: 1,
        values: Object.keys(addressJson),
        className: 'slot1',
        textAlign: 'center'
      }, {
        divider: true,
        content: '-',
        className: 'slot2'
      }, {
        flex: 1,
        values: ['市辖区'],
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
        values: ['东城区'],
        className: 'slot5',
        textAlign: 'center'
      }
    ],
    province:'省',
    city:'市',
    county:'区/县',
    idVisible: false,
    myId: [{
        values: [
          {name:'请选择',value:'请选择'}, 
          {name:'身份证',value:'身份证'}, 
          {name:'军官证',value:'军官证'},
          {name: '护照',value:'护照'}]
          }],
    id: 'id'
}},
  methods: {
    clickitem (e) {
      e === this.ruleForm.radio ? this.ruleForm.radio = '' : this.ruleForm.radio = e
    },
    getDate: function(){
      if(this.ruleForm.phone!=""){
          if(!(/^1[3456789]\d{9}$/.test(this.ruleForm.phone))){ 
            this.$message({
              showClose: true,
              message: '手机号码有误，请重填!',
              type: 'warning',
              offset: '300px'
            });
            return
        } 
      }
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
      }else if(this.ruleForm.address==""){
        this.$message({
          showClose: true,
          message: '家庭住址不能为空!',
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
      }
    },
    onAddressChange(picker, values) {
      if(addressJson[values[0]]) {
        picker.setSlotValues(1, Object.keys(addressJson[values[0]]));
        picker.setSlotValues(2, addressJson[values[0]][values[1]]);
        this.province = values[0];
        this.city = values[1];
        this.county = values[2];
      }
    },
    onIdChange(picker,values){
        this.id = picker.getValues()[0].value;
    },
    showAddressPicker(){
      this.regionVisible = true;
    },
    showIdPicker(){
      this.idVisible = true;
    },
    handleFinishAddress(){
      this.ruleForm.address = this.province + this.city+this.county;
      this.regionVisible = false;
    },
    handleFinishId(){
      this.ruleForm.type = this.id; 
      this.idVisible = false;
    },
    handleCloseAddress(){
      this.regionVisible = false;
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
          .el-radio-group{
            width: 100%;
            .el-radio{
              width: auto;
            }
          }
          .el-radio{
            width:100%;
          }
          .mint-popup{
            width: 100%;
            height: 40%;
            .cateContainer{
              .operateBtn{
                height: 55px;
                background: #eee;
                .btn{
                  color: blue;
                  line-height: 55px;
                  font-size: 16px;
                  cursor: pointer;
                }
                .cancelBtn {
                  float: left;
                  margin-left: 80px;
                }
                .confirmBtn{
                  float: right;
                  margin-right: 80px;
                }
              }
            }
          }
          .setInput{
            display: block;
            width: 86%;
            height: 38px;
            border: 1px solid #DCDFE6;
            border-radius: 4px;
            padding: 0 14px;
          }
          .setInput::placeholder{
           font-size: 14px;
           color: #c0c4cc;
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
