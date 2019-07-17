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
          <el-input v-model="formDate.sex"></el-input>
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
      formDate:{
        date:'',
        name:'',
        email:'',
        title:'',
        evaluate:'',
        state:''
      },
      formrules:{
        // date:[{required:true,message:"日期不能为空",trigger:"blur"}],
        name:[{required:true,message:"用户名不能为空",trigger:"blur"}],
      }
    }
  },
  methods:{
    creatRefresh(response) {
      console.log(response)
      // this.getStuffInfoData()
    },
    dialogFormAdd(formdong) {
        // this.$refs[formdong].validate((valid) => {
        //   if (valid) {
        //     this.$axios.post('http://localhost:8081/yxxtcs/student.php',this.formDate).then(res => {
        //         this.$message({
        //             type:"success",
        //             message:"添加信息成功"
        //         })
        //         this.dialogAdd.show = false;
        //         this.$emit('update');
 
        //     })
        //     this.formDate  = ""
        //   } else {
        //     console.log('error submit!!');
        //     return false;
        //   }
        // })
         var fd = new FormData()
        fd.append("name",this.formDate.name)
        fd.append("province",this.formDate.province)
        fd.append("num",this.formDate.num)
        fd.append("sex",this.formDate.sex)
        fd.append("message",this.formDate.message)
        fd.append("xueyuan",this.formDate.xueyuan)
        fd.append("dorm",this.formDate.dorm)
        fd.append("zy",this.formDate.zy)
        fd.append("address",this.formDate.address)
        fd.append("code",this.formDate.code)
        fd.append("phone",this.formDate.phone)
        fd.append("receive",this.formDate.receive)
        fd.append("result",this.formDate.result)
        // console.log(fd)
        axios.post(`http://localhost:8081/yxxtcs/student.php`,fd).then(this.creatRefresh)
        this.$message({
          type: 'success',
          message: '新建成功!'
        });
    }
  }
}
</script>
 
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
   
</style>