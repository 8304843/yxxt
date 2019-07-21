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
        name:[{required:true,message:"用户名不能为空",trigger:"blur"}],
        province:[{required:true,message:"省份不能为空",trigger:"blur"}],
        num:[{required:true,message:"考生号不能为空",trigger:"blur"}],
        message:[{required:true,message:"身份证号不能为空",trigger:"blur"}],
        xueyuan:[{required:true,message:"二级学院不能为空",trigger:"blur"}],
        zy:[{required:true,message:"录取专业不能为空",trigger:"blur"}],
        address:[{required:true,message:"邮寄地址不能为空",trigger:"blur"}],
        code:[{required:true,message:"邮政编码不能为空",trigger:"blur"}],
        phone:[{required:true,message:"联系电话不能为空",trigger:"blur"}],
        receive:[{required:true,message:"收件人不能为空",trigger:"blur"}],
        result:[{required:true,message:"投档成绩不能为空",trigger:"blur"}],
      }
    }
  },
  methods:{
    creatRefresh(response) {
      console.log(response)
    },
    dialogFormAdd(formdong) {
        var fd = new FormData()
        fd.append("name",this.formDate.name)
        fd.append("province",this.formDate.province)
        fd.append("num",this.formDate.num)      
        fd.append("message",this.formDate.message)
        fd.append("xueyuan",this.formDate.xueyuan)
        fd.append("dorm",this.formDate.dorm)
        fd.append("zy",this.formDate.zy)
        fd.append("address",this.formDate.address)
        fd.append("code",this.formDate.code)
        fd.append("phone",this.formDate.phone)
        fd.append("receive",this.formDate.receive)
        fd.append("result",this.formDate.result)
        if(this.sex==0){
          this.sex = '男'
          fd.append("sex",this.sex)
        }else{
          this.sex = '女'
          fd.append("sex",this.sex)
        }
        if(this.payment==0){
          this.payment = '已缴费'
          fd.append("payment",this.payment)
        }else{
          this.payment = '未缴费'
          fd.append("payment",this.payment)
        }

        this.$refs[formdong].validate((valid) => {
          if (valid) {
            axios.post(`http://localhost:8081/yxxtcs/student.php`,fd).then(this.creatRefresh)
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