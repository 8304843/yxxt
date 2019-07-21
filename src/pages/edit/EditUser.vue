<template>
  <div class="hello">
    <el-dialog title="编辑学生信息" :modal-append-to-body='false' :visible.sync="dialogEdit.show">
      <el-form :model="form" ref="formEdit" label-width="100px" :rules="formrules">
        <el-form-item label="姓名" prop="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="省份" prop="province">
          <el-input v-model="form.province"></el-input>
        </el-form-item>
        <el-form-item label="考生号" prop="num">
          <el-input v-model="form.num"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="sex">
          <el-input v-model="form.sex"></el-input>
        </el-form-item>
        <el-form-item label="身份证号" prop="message">
          <el-input v-model="form.message"></el-input>
        </el-form-item>
        <el-form-item label="二级学院" prop="xueyuan">
          <el-input v-model="form.xueyuan"></el-input>
        </el-form-item>
        <el-form-item label="宿舍号" prop="dorm">
          <el-input v-model="form.dorm"></el-input>
        </el-form-item>
         <el-form-item label="缴费情况" prop="payment">
          <el-select v-model="form.payment" style="width:100%;" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="录取专业" prop="zy">
          <el-input v-model="form.zy"></el-input>
        </el-form-item>
        <el-form-item label="邮寄地址" prop="address">
          <el-input v-model="form.address"></el-input>
        </el-form-item>
        <el-form-item label="邮政编码" prop="code">
          <el-input v-model="form.code"></el-input>
        </el-form-item>
        <el-form-item label="联系电话" prop="phone">
          <el-input v-model="form.phone"></el-input>
        </el-form-item>
        <el-form-item label="收件人" prop="receive">
          <el-input v-model="form.receive"></el-input>
        </el-form-item>
        <el-form-item label="投档成绩" prop="result">
          <el-input v-model="form.result"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogEdit.show = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormEdit('formEdit')">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
 
<script>
import axios from "axios";

export default {
  name: 'HelloWorld',
  props:{
    dialogEdit:Object,
    form:Object
  },
  data () {
    return {
      options: [{
          value: '已缴费',
          label: '已缴费'
        }, {
          value: '未缴费',
          label: '未缴费'     
        }],
        value: '',
      formrules:{
        name:[{required:true,message:"用户名不能为空",trigger:"blur"}],
        province:[{required:true,message:"省份不能为空",trigger:"blur"}],
        num:[{required:true,message:"考生号不能为空",trigger:"blur"}],
        sex:[{required:true,message:"性别不能为空",trigger:"blur"}],
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
    dialogFormEdit(formEdit) {
      var fd = new FormData()
      fd.append("name",this.form.name)
      fd.append("id",this.form.id)
      fd.append("province",this.form.province)
      fd.append("num",this.form.num)
      fd.append("sex",this.form.sex)
      fd.append("message",this.form.message)
      fd.append("xueyuan",this.form.xueyuan)
      fd.append("dorm",this.form.dorm)
      fd.append("zy",this.form.zy)
      fd.append("address",this.form.address)
      fd.append("code",this.form.code)
      fd.append("phone",this.form.phone)
      fd.append("receive",this.form.receive)
      fd.append("result",this.form.result)
      fd.append("payment",this.form.payment)
        this.$refs[formEdit].validate((valid) => {
          if (valid) {
            this.$axios.post(`http://localhost:8081/yxxtcs/Mes_Change.php`,fd).then(res => {
                this.$message({
                    type:"success",
                    message:"编辑信息成功"
                })
              console.log(res)
                this.dialogEdit.show = false;
                this.$emit('updateEdit')
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        })
    }
  }
}
</script>
 
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
   
</style>