<template>
  <div class="hello">
    <el-dialog title="添加学生信息" :modal-append-to-body='false' :visible.sync="dialogAdd.show">
      <el-form :model="formDate" ref="formdong" label-width="100px" :rules="formrules">
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
            <!-- <img :src="'http://localhost:8081/yxxtcs/upload/'+form.photo" style="position:absolute ;top:1px;left:200px;" class="avatar">  -->
          </template>
        </el-form-item>
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
        <el-form-item label="班级信息" prop="classmate">
          <el-input v-model="formDate.classmate"></el-input>
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
        <el-form-item label="注册状态" prop="registe">
        <el-select v-model="registe" style="width:100%;" placeholder="请选择">
          <el-option
            v-for="item in options2"
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
        <el-button @click="dialogFormClose('formEdit')">取 消</el-button>
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
      list:[],
      imageUrl:'',
      userId:'0',
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
        classmate : '' 
      },
      options: [{
          value: '0',
          label: '已缴费'
        }, {
          value: '1',
          label: '未缴费'     
        }],
      options1: [{
        value: '1',
        label: '男'
      }, {
        value: '0',
        label: '女'     
      }],
      options2: [{
          value: '0',
          label: '已注册'
        }, {
          value: '1',
          label: '未注册'     
        }],
        payment: '0',
        sex:'1',
        registe:'1',
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
        classmate:[{required:true,message:"班级信息不能为空",trigger:"blur"}],
      }
    }
  },
  methods:{
    creatRefresh(response) {
      console.log(response)
    },
    dialogFormAdd(formdong) {
        var fd  = new FormData()
        fd.append("flag",'Add')
        fd.append("name",this.formDate.name)
        fd.append("province",this.formDate.province)
        fd.append("num",this.formDate.num)      
        fd.append("message",this.formDate.message)
        fd.append("xueyuan",this.formDate.xueyuan)
        fd.append("zy",this.formDate.zy)
        fd.append("address",this.formDate.address)
        fd.append("code",this.formDate.code)
        fd.append("phone",this.formDate.phone)
        fd.append("receive",this.formDate.receive)
        fd.append("result",this.formDate.result)
        fd.append("dorm",this.formDate.dorm)
        fd.append("classmate",this.formDate.classmate)
        if(this.sex==1){
          fd.append("sex",'男')
        }else{
          fd.append("sex",'女')
        }
        if(this.payment==0){
          this.payment = '已缴费'
          fd.append("payment",this.payment)
        }else{
          this.payment = '未缴费'
          fd.append("payment",this.payment)
        }
        if(this.registe==0){
          this.registe = '已注册'
          fd.append("registe",this.registe)
        }else{
          this.registe = '未注册'
          fd.append("registe",this.registe)
        }
        var picture_url = localStorage.getItem('photo_base64')
        let token =localStorage.getItem('my_token')
        // picture_url = picture_url.replace(/^data:image\/\w+;base64,/, "")  
        this.$refs[formdong].validate((valid) => {
          if (valid) {
            if(picture_url==null){
              // console.log('不调用接口')
              fd.append("state",'未上传')
              fd.append('file', picture_url)
              axios.post(`http://localhost:8081/yxxtcs/Pic_Upload_Add.php`,fd).then(res => {
                if(res.data.states=='已存在'){
                  this.$message({
                    type: 'error',
                    message: '人员已存在，请检查考生号！'
                  });
                }else{
                  this.$message({
                    type: 'success',
                    message: '新建成功!'
                  }); 
                  this.dialogAdd.show = false;           
                  clearTimeout(this.timer);  //清除延迟执行 
                  this.timer = setTimeout(()=>{   //设置延迟执行
                    // console.log('ok');
                    this.$emit('update');
                  },100)
                  this.empty()
                }
              })
            }else{
              // console.log('调用接口上传人员信息')
              // console.log('更新照片')
              fd.append("state",'已上传')
              var picture_Base64 = picture_url.replace(/^data:image\/\w+;base64,/, "")
              fd.append('file', picture_url)
              fd.append('Base64', picture_Base64)
              var workCode = this.formDate.num
              var name = this.formDate.name
              axios.post(`http://localhost:8081/yxxtcs/Pic_Upload_Add.php`,fd).then(res => {
                if(res.data.states=='已存在'){
                  this.$message({
                    type: 'error',
                    message: '人员已存在，请检查考生号！'
                  });
                }else{
                  this.dialogAddInterface(picture_url,token,workCode,name)//调用接口上传
                  this.$message({
                    type: 'success',
                    message: '新建成功!'
                  }); 
                  this.dialogAdd.show = false;           
                  clearTimeout(this.timer);  //清除延迟执行 
                  this.timer = setInterval(()=>{   //设置延迟执行
                    // console.log('ok');
                    this.$emit('update');
                  },100)
                  this.empty()
                }
              })
            }
          }else {
            console.log('error submit!!');
            return false;
          }
        })
        localStorage.removeItem('photo_base64')
    },
    dialogAddInterface(picture_url,token,workCode,name){//调用添加人员接口
      picture_url = picture_url.replace(/^data:image\/\w+;base64,/, "")
      this.list = []
      var fd = new Array()
      var fd = [
          {
            id:"55", //人员编号 
            name:name,//人员姓名
            workCode:workCode, //人员工号 系统全局唯一编号，不能重复 
            orgCode:"1000", //机构编码 
            deptCode:"1000", //部门编码 
            gender:this.sex, //性别 1：男 0：女 
            userType:"1", //人员类型 1：普通员工 2：VIP 3：黑名单 
            picture:picture_url, //注册照片 
            customerPicture:picture_url, //显示照片 
            devIds:[""] //需要同步的设备列表 
          }
          ]
        this.list = fd
        axios.post(`api/user/public/api/v1.0/add?token=${token}`,{
          userList:this.list
        }).then(res=>{
          this.userId = res.data.data[0].userId
          var userId = this.userId
          this.update_userId(userId,workCode)
        })
        this.$message({
          type: 'success',
          message: '新建成功!'
        }); 
    },
    dialogFormClose(formEdit){
      this.dialogAdd.show = false;
      this.imageUrl = '';
    },
    update_userId(userId,workCode){
      var fd  = new FormData()
      fd.append("userId",userId)
      fd.append("num",workCode)
      axios.post(`http://localhost:8081/yxxtcs/Mes_UserId.php`,fd).then(res=>{
        console.log(res)
      })
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
      return isJPG && isLt2M;
    },
    empty() {
      this.formDate.name=''
      this.formDate.province=''
      this.formDate.num=''
      this.formDate.message=''
      this.formDate.xueyuan=''
      this.formDate.dorm=''
      this.formDate.zy=''
      this.formDate.address=''
      this.formDate.code=''
      this.formDate.phone=''
      this.formDate.receive=''
      this.formDate.result=''
      this.imageUrl = '';
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