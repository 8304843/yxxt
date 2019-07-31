<template>
  <div class="hello">
    <el-dialog title="编辑学生信息" :modal-append-to-body='false' :visible.sync="dialogEdit.show">
      <el-form :model="form" ref="formEdit" label-width="100px" :rules="formrules">
        <el-form-item label="头像" prop="photo">
         <template slot-scope="scope">
            <el-upload
              class="avatar-uploader"
              action="http://localhost:8081/yxxtcs/Pic_Upload.php"
              :show-file-list="false"
              :on-success="handleAvatarSuccess"
              :before-upload="beforeAvatarUpload">
              <img v-if="imageUrl" :src="imageUrl" class="avatar">             
              <!-- <el-button size="small" type="primary">点击上传</el-button> -->
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
              <!-- <i v-else>上传预览</i> -->
            </el-upload>   
            <img :src="'http://localhost:8081/yxxtcs/upload/'+form.photo" style="position:absolute ;top:1px;left:200px;" class="avatar"> 
          </template>
        </el-form-item>
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
        <el-button @click="dialogFormClose('formEdit')">取 消</el-button>
        <el-button type="primary" @click="dialogFormEdit('formEdit')">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template> 
 
<script>
import axios from "axios";

export default {
  inject:['reload'],
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
        flag:'null',//用以判断是否点击上传图片
        photo_url:'',
        imageUrl: ``,
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
  created () {

  },
  methods:{
    dialogFormEdit(formEdit) {  
      var fd = new FormData()
      fd.append("flag",'Edit')
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
                if(localStorage.getItem('photo_base64')==null){
                  // console.log('不更新照片')
                }else{
                  // console.log('更新照片')
                  fd.append('file', localStorage.getItem('photo_base64'))
                  axios.post(`http://localhost:8081/yxxtcs/Pic_Upload.php`,fd).then(res => {
                    console.log(res.data)
                  })
                }
                this.imageUrl = '';
                console.log(res)
                this.dialogEdit.show = false;
                this.$emit('updateEdit')
                localStorage.removeItem('photo_base64')
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        })
    },
    dialogFormClose(formEdit){
      this.dialogEdit.show = false;
      this.imageUrl = '';
    },
    handleAvatarSuccess(res, file) {
      this.imageUrl = URL.createObjectURL(file.raw);
    },
    beforeAvatarUpload(file) {
      //上传前对图片类型和大小进行判断
      const isJPG = file.type === 'image/jpeg';
      const isLt2M = file.size / 1024 / 1024 < 2;
      if (!isJPG) {
        this.$message.error('上传头像图片只能是 JPG 格式!');
      }
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能超过 2MB!');
      }
      //校验成功上传文件
      if(isJPG && isLt2M == true){
        console.log(file);
        // post上传图片
        new Promise(function (resolve, reject) {
          var reader = new FileReader();
          reader.readAsDataURL(file);
          reader.onload = (e) => {
            // 读取到的图片base64 数据编码 将此编码字符串传给后台即可
            var imgcode = e.target.result; 
            localStorage.setItem('photo_base64',imgcode)
          }
        })  
        this.$message({
          type:"success",
          message:"上传成功，请按确定以保存！"
        })  
      }
      // this.$emit('updateEdit')
      this.reload()
      return isJPG && isLt2M;
    }
  }
}
</script>
 
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 108px;
    height: 108px;
    line-height: 108px;
    text-align: center;
  }
  .avatar {
    width: 108px;
    height: 108px;
    display: block;
  }
</style>