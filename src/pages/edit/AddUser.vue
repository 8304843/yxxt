<template>
  <div class="hello">
    <el-dialog title="添加学生信息" :modal-append-to-body='false' :visible.sync="dialogAdd.show">
      <el-form :model="formDate" ref="formdong" label-width="100px" :rules="formrules">
        <!-- <el-form-item label="日期" prop="date">
            <el-date-picker
              v-model="formDate.date"
              type="date"
              placeholder="选择日期">
            </el-date-picker>
        </el-form-item> -->
        <el-form-item label="姓名" prop="name">
          <el-input v-model="formDate.name"></el-input>
        </el-form-item>
        <el-form-item label="省份" prop="province">
          <el-input v-model="formDate.province"></el-input>
        </el-form-item>
        <el-form-item label="考生号" prop="num">
          <el-input v-model="formDate.num"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="sex">
        <el-select v-model="sex" style="width:100%;" placeholder="请选择">
          <el-option
            v-for="item in options1"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="身份证号" prop="message">
          <el-input v-model="formDate.message"></el-input>
        </el-form-item>
        <el-form-item label="二级学院" prop="xueyuan">
          <el-input v-model="formDate.xueyuan"></el-input>
        </el-form-item>
        <el-form-item label="宿舍号" prop="dorm">
          <el-input v-model="formDate.dorm"></el-input>
        </el-form-item>
        <el-form-item label="缴费情况" prop="payment">
        <el-select v-model="payment" style="width:100%;" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="录取专业" prop="zy">
          <el-input v-model="formDate.zy"></el-input>
        </el-form-item>
        <el-form-item label="邮寄地址" prop="address">
          <el-input v-model="formDate.address"></el-input>
        </el-form-item>
        <el-form-item label="邮政编码" prop="code">
          <el-input v-model="formDate.code"></el-input>
        </el-form-item>
        <el-form-item label="联系电话" prop="phone">
          <el-input v-model="formDate.phone"></el-input>
        </el-form-item>
        <el-form-item label="收件人" prop="receive">
          <el-input v-model="formDate.receive"></el-input>
        </el-form-item>
        <el-form-item label="投档成绩" prop="result">
          <el-input v-model="formDate.result"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogAdd.show = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormAdd('formdong')">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
 
<script>
import axios from "axios";
export default {
  name: 'AddUser',
  props:{
    dialogAdd:Object
  },
  data () {
    return {
      restaurants: [],
      formDate:{
        name:'',//姓名
        province:'',//省份
        num:'',//考生号
        sex:'',//性别
        message:'',//身份证号
        xueyuan:'',//二级学院
        dorm:'',//宿舍号
        zy : '',//专业
        address :'',//邮寄地址
        code :'',//邮政编码
        phone :'',//联系电话
        receive :'',//收件人
        result : '',//投档成绩
        payment : '',
      },
      options: [{
          value: '0',
          label: '已缴费'
        }, {
          value: '1',
          label: '未缴费'     
        }],
      options1: [{
        value: '0',
        label: '男'
      }, {
        value: '1',
        label: '女'     
      }],
        payment: '0',
        sex:'0',
      formrules:{
        // name:[{required:true,message:"用户名不能为空",trigger:"blur"}],
        // province:[{required:true,message:"省份不能为空",trigger:"blur"}],
        // num:[{required:true,message:"考生号不能为空",trigger:"blur"}],
        // message:[{required:true,message:"身份证号不能为空",trigger:"blur"}],
        // xueyuan:[{required:true,message:"二级学院不能为空",trigger:"blur"}],
        // zy:[{required:true,message:"录取专业不能为空",trigger:"blur"}],
        // address:[{required:true,message:"邮寄地址不能为空",trigger:"blur"}],
        // code:[{required:true,message:"邮政编码不能为空",trigger:"blur"}],
        // phone:[{required:true,message:"联系电话不能为空",trigger:"blur"}],
        // receive:[{required:true,message:"收件人不能为空",trigger:"blur"}],
        // result:[{required:true,message:"投档成绩不能为空",trigger:"blur"}],
      }
    }
  },
  methods:{
    creatRefresh(response) {
      console.log(response)
    },
    dialogFormAdd(formdong) {
        var userList  = new FormData()
        userList.append("name",'1')
        userList.append("workCode",'2017010302113')
        userList.append("orgCode",'0001')      
        userList.append("deptCode",'0002')
        userList.append("gender",'1')
        userList.append("userType",'1')
        userList.append("picture",'data:image/jpeg;base64,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')
        userList.append("customerPicture",'')
        userList.append("devIds",'["1", "2"]')
        // fd.append("phone",this.formDate.phone)
        // fd.append("receive",this.formDate.receive)
        // fd.append("result",this.formDate.result)
        // if(this.sex==0){
        //   this.sex = '男'
        //   fd.append("sex",this.sex)
        // }else{
        //   this.sex = '女'
        //   fd.append("sex",this.sex)
        // }
        // if(this.payment==0){
        //   this.payment = '已缴费'
        //   fd.append("payment",this.payment)
        // }else{
        //   this.payment = '未缴费'
        //   fd.append("payment",this.payment)
        // }

        this.$refs[formdong].validate((valid) => {
          if (valid) {
            axios.post(`api/user/public/api/v1.0/add?token=ff9666ea-67f1-447c-a269-649c733ea7c8`,userList).then(this.creatRefresh)
            this.$message({
              type: 'success',
              message: '新建成功!'
            }); 
            this.dialogAdd.show = false;
            this.$emit('update');
            this.empty()
          }else {
              console.log('error submit!!');
              return false;
            }
        })
    },
    empty() {
      this.formDate.name=''
      this.formDate.province=''
      this.formDate.num=''
      this.sex=''
      this.formDate.message=''
      this.formDate.xueyuan=''
      this.formDate.dorm=''
      this.formDate.zy=''
      this.formDate.address=''
      this.formDate.code=''
      this.formDate.phone=''
      this.formDate.receive=''
      this.formDate.result=''
      this.payment=''
    },
  },
  mounted() {
   
  }
}
</script>
 
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
   .el-select .el-input {
    width: 130px;
  }
</style>