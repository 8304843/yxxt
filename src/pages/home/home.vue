<%--
@Project Name: vue-admin
@Author: luichooy
@Date: 2017-11-16 12:27
@Email: luichooy@163.com
@Idea: WebStorm
--%>

<template>
  <div class='basic'>
    <el-row>
      <el-col :span='24'>
        <el-card>
          <div slot="header">
            <el-button type="primary" title="添加学生信息" size="" icon="el-icon-edit-outline" @click="hanldeAdd()">添加</el-button>
            <el-button type="primary" title="导入学生信息" size="" icon="el-icon-edit-outline" @click="enter()">导入</el-button>
            <el-input  style="width:200px;left:10px" title="可按姓名、学号、专业、宿舍信息、缴费情况查找"  v-model="keyUser"  placeholder="查询所需要的内容......"></el-input>
          </div>
          <div class="table">
            <el-table
              v-loading="loading"
              element-loading-text="加载数据中"
              height="650"
              :data="searchUserinfo(keyUser)"
              border>
              <el-table-column
                type="index"
                label="序号"
                align="center"
                width="60">
              </el-table-column>
              <el-table-column  label="姓名" prop="name" align="center" width="120%"></el-table-column>
              <el-table-column label="头像" prop="state" align="center" width="120" height="200">              
              </el-table-column>
              <el-table-column label="考生号" prop="num" align="center" width="190%" header-align="center"></el-table-column>
              <el-table-column label="二级学院" prop="xueyuan" align="center" header-align="center" width="170%"></el-table-column>  
              <el-table-column label="录取专业" prop="zy" align="center" header-align="center" width="190%"></el-table-column>   
              <el-table-column label="宿舍信息" prop="dorm" align="center" header-align="center" width="180%"></el-table-column> 
              <el-table-column label="缴费情况" prop="payment" align="center" width="120%"></el-table-column>
              <el-table-column label="录入时间" prop="date" align="center" width="170%"></el-table-column> 
              <el-table-column label="操作" align="center" width="250%">
              <template slot-scope="scope">
                <el-button
                  size="mini"
                   @click="print(scope.$index, scope.row)"
                 >打印</el-button>
                <el-button
                  size="mini"
                  @click="handleEdit(scope.$index, scope.row)"
                  >编辑</el-button>
                <el-button
                  size="mini" 
                  type="danger"
                  @click="handleDelete(scope.$index, scope.row)">删除</el-button>
              </template>
            </el-table-column>
            </el-table>
            <el-pagination
              style="margin-top: 16px; text-align:right;"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="paginations.page_index"
              :page-sizes="paginations.page_sizes"
              :page-size="paginations.page_size"
              :layout="paginations.layout"
              :total="paginations.total">
            </el-pagination>
          </div> 
        </el-card>
      </el-col>
    </el-row>
    <AddUser :dialogAdd="dialogAdd" @update="Mes_Show"></AddUser>
    <EditUser :dialogEdit="dialogEdit" :form="form" @updateEdit="Mes_Show"></EditUser>
     <router-view v-if="isRouterAlive"></router-view>
    <el-dialog title="人员导入" :visible.sync="dialogFormUpload" :modal-append-to-body='false' width="25%">
        <el-upload
          :on-success="success"
          :on-exceed="fileExceed"
          :on-change="onchangeFunc"
          drag
          :auto-upload="false"
          ref="upload"
          :limit="1"
          :action="target"
          multiple>
          <i class="el-icon-upload"></i>
          <div class="el-upload__text" width="100%">将文件拖到此处，或<em>点击上传</em></div>
          <div class="el-upload__tip" slot="tip">只能上传xlsx、lsx文件</div>
        </el-upload>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormUpload = false">取 消</el-button>
         <el-button type="primary" @click="upload()">导 入</el-button>
        </div>
    </el-dialog>
  </div>
</template>
<script>
  import EditUser from '../edit/EditUser';
  import AddUser from '../edit/AddUser';
  import axios from "axios";

  const POSITIVE = 0;
  const NEGATIVE = 1;
  export default {
    name : 'basic',
    provide() {
        return {
            reload: this.reload
        }
    },
    data () {
      return {
        isRouterAlive: true,
        dialogFormUpload:false,
        target:`http://localhost:8081/yxxtcs/people_upload.php`,
        tableData: [],
        loading: true,
        pagesize: 10,
        currentpage: 1,
        total: 0,
        keyUser:'',
        dialogEdit:{
          show:false,
        },
        dialogAdd:{
          show:false
        },
				paginations:{
					page_index:1, //当前页
					total:0, //总数
					page_size:10, //一页显示多少
					page_sizes:[5,10,15,20], //每页显示多少条
					layout:'total, sizes, prev, pager, next, jumper'
        },
        imageUrl: '',
				allTableData:[],
        form:{   //编辑信息
          id:'',
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
          payment:'',//缴费情况
          date:'',//录入时间
          photo: '',//照片
        },
      }
    },
    created () {
      this.Mes_Show();
      this.show();
    },
    methods: {
      Mes_Show (scope) {
        axios.post(`http://localhost:8081/yxxtcs/Mes_Show.php`).then((res)=> {
          // console.log(res.data.data)
          this.tableData = res.data.data
          this.total = res.data.data.length-1;
          this.loading = false;  
          const data = res.data.data;
          this.allTableData = data;
          this.setPaginations()
        }) 
      },
      reload () {
          this.isRouterAlive = false;
          this.$nextTick(function() {
              this.isRouterAlive = true;
          })
      },
      show(){
        let token =localStorage.getItem('my_token')
        console.log(token)
        axios.post(`/api/user/public/api/v1.0/list?token=${token}`).then((res)=> {//调用接口获取人员列表
          console.log(res.data)
        }) 
      },
      setPaginations(){
				this.paginations.total = this.allTableData.length; //数据的数量
				this.paginations.page_index = 1; //默认显示第一页
				this.paginations.page_size = 10; //每页显示多少数据
				
				//显示数据
				this.tableData = this.allTableData.filter((item,index) => {
					return index < this.paginations.page_size;
				})
			},
      handleEdit(index,row){ //编辑
        this.dialogEdit.show = true; //显示弹
        this.form = {
          province:row.province,
          num:row.num,
          id:row.id,
          name:row.name,
          sex:row.sex,
          message:row.message,
          xueyuan:row.xueyuan,
          dorm:row.dorm,
          zy : row.zy,
          address :row.address,
          code :row.code,
          phone :row.phone,
          receive :row.receive,
          result : row.result,
          payment: row.payment,
          photo: row.photo
        }
      },
      hanldeAdd(){  //添加
        this.dialogAdd.show = true;
      },
      handleDelete(index, row) {//删除
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          var fd = new FormData()
          fd.append("id",row.id)
          this.$axios.post(`http://localhost:8081/yxxtcs/Mes_Del.php`,fd).then(res =>{
            this.$message({
                type:"success",
                message:"删除信息成功"
            })
            this.Mes_Show()    //删除数据，更新视图
        })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      },
       searchUserinfo(keyUser) {
        return this.tableData.filter((user) => {
            if(user.name.includes(keyUser)) {//按姓名查找
                return user
            }
            if(user.num.includes(keyUser)) {//按学号查找
                return user
            }
            if(user.payment.includes(keyUser)) {//按缴费情况查找
                return user
            }
            if(user.dorm.includes(keyUser)) {//按宿舍信息查找
                return user
            }
            if(user.zy.includes(keyUser)) {//按录取专业查找
                return user
            }
            if(user.date.includes(keyUser)) {//按录入时间查找
                return user
            }
            if(user.sex.includes(keyUser)) {//按性别查找
                return user
            }
        })
    },
    print(index,row) {
      // alert(row.province);
      let routeData = this.$router.resolve({
        path: '/printTable',
        query:{
          province:row.province,
          num:row.num,
          id:row.id,
          name:row.name,
          sex:row.sex,
          message:row.message,
          xueyuan:row.xueyuan,
          dorm:row.dorm,
          zy : row.zy,
          address :row.address,
          code :row.code,
          phone :row.phone,
          receive :row.receive,
          result : row.result,
          payment: row.payment

        }
      });
      window.open(routeData.href, '_blank');
    },
      //选择文件时触发
    onchangeFunc(file,fileList){
      console.log(fileList)
      this.fileList = fileList
    },
    //导入按钮
    enter(){
      this.dialogFormUpload = true
    },
    //导入按钮
    upload(){
      this.dialogFormUpload = false
      if(this.$refs.upload.uploadFiles){
        this.$refs.upload.submit() // 上传
        //获取本地数据库数据，添加到接口
        this.Mes_Show();
      } else {
        this.$message({
          type: 'warning',
          message: '您未添加任何文件！'
        });
      }
    },
    // 超出上传限制提示框
    fileExceed (e) {
      let options = {
        message: '已添加文件！',
        type: 'warning',
        duration: 2000,
      }
      Message(options);
    },
    // 上传成功回调
    success (res) {
      // this.$emit('uploadSuccess',res);
      console.log(res)
      this.Mes_Show()
    },
    handleSizeChange (page_size) {
      this.paginations.page_index = 1; //第一页
				this.paginations.page_size = page_size; //每页先显示多少数据
				this.tableData = this.allTableData.filter((item,index) => {
					return index < page_size
				})
    },
    handleCurrentChange (page) {
      // 跳转页数
				//获取当前页
				let index = this.paginations.page_size * (page -1);
				//获取总数
				let allData = this.paginations.page_size * page;
				
				let tablist=[];
				for(let i = index;i<allData;i++) {
					if (this.allTableData[i]) {
						tablist.push(this.allTableData[i])
					}
					this.tableData = tablist
				}
      },
      addRowClass ({row, rowIndex}) {
      if (row.rateType === NEGATIVE) {
        return 'warning-row';
      }
    },
},
  
  components: {
     // score
    EditUser,
     AddUser
  }
};

</script>
<style lang='scss' scoped>
h1{
    font-size: 30px;
    color: #333;
    text-align: center;
    margin: 0 auto;
    padding-bottom: 5px;
    border-bottom: 2px solid #409EFF;
    width: 300px
}
  .basic {
    .el-table {
      .warning-row {
        background-color: oldlace;
      } 
    }

    .recommend-tag {
      display: inline-block;
      margin: 4px 0;
      margin-right: 4px;

      &:last-child {
        margin-right: 0;
      }
    }
  }
</style>


