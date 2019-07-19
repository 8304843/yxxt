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
            <el-button type="primary" title="导入学生信息" size="" icon="el-icon-edit-outline" >导入</el-button>
            <el-input  style="width:200px;left:10px" placeholder="查询所需要的内容......"></el-input>
          </div>
          <div class="table">
            <el-table
              v-loading="loading"
              element-loading-text="加载数据中"
              :data='tableData'
              height="650"
              border>
              <el-table-column
                type="index"
                label="序号"
                align="center"
                width="60">
              </el-table-column>
              <el-table-column  label="姓名" prop="name" align="center" width="120">
                <template slot-scope="scope">
                    <span>{{ scope.row.name }}</span>
                </template>
              </el-table-column>
              <el-table-column label="头像" align="center" width="120">
                <template slot-scope="scope">
                  <img :src="scope.row.avatar" alt="用户头像" width="42" height="42" style="border-radius: 50%;">
                </template>
              </el-table-column>
              <el-table-column label="宿舍信息" align="center" header-align="center" width="210">
                <template  slot-scope="scope">
                  <span>{{ scope.row.name}}</span>
                </template>
              </el-table-column>  
              <el-table-column label="问候语" prop="text" align="left" width="" header-align="center"></el-table-column>
              <el-table-column label="缴费情况" align="center" width="110%">
                <template slot-scope="scope">{{scope.row.rateType | rateTypeToText}}</template>
              </el-table-column>
              <el-table-column label="录入时间" align="center" width="180%">
                <template slot-scope="scope">
                  <span>{{ scope.row.rateTime | formatDate }}</span>
                </template>
              </el-table-column> 
              <el-table-column label="操作" align="center" width="250">
              <template slot-scope="scope">
                <el-button
                  size="mini"
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
              layout="total, sizes, prev, pager, next, jumper"
              :page-sizes="[5, 10, 15, 20, 25]"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :total="total">
            </el-pagination>
          </div> 
        </el-card>
      </el-col>
    </el-row>
    <AddUser :dialogAdd="dialogAdd" @update="Mes_Show"></AddUser>
    <EditUser :dialogEdit="dialogEdit" :form="form" @updateEdit="Mes_Show"></EditUser>
  </div>
</template>
<script>
  import {formatDate} from 'src/utils/utils';
  import EditUser from '../edit/EditUser';
  import AddUser from '../edit/AddUser';
  import axios from "axios";

  const POSITIVE = 0;
  const NEGATIVE = 1;
  export default {
    name : 'basic',
    data () {
      return {
        tableData: [],
        loading: true,
        pagesize: 10,
        currentpage: 1,
        total: 0,
        dialogEdit:{
          show:false,
        },
        dialogAdd:{
          show:false
        },
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
        },
      }
    },
    methods: {
      Mes_Show (scope) {
        axios.post(`http://localhost:8081/yxxtcs/Mes_Show.php`).then((res)=> {
          this.tableData = res.data.data
          this.loading = false;            
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
        }
        this.Mes_Show();
      },
      hanldeAdd(){  //添加
        this.dialogAdd.show = true;
      },
      handleDelete(index, row) {//删除
      var fd = new FormData()
      fd.append("id",row.id)
        this.$axios.post(`http://localhost:8081/yxxtcs/Mes_Del.php`,fd).then(res =>{
            this.$message({
                type:"success",
                message:"删除信息成功"
            })
            this.Mes_Show()    //删除数据，更新视图
        })
      },
      handleSizeChange (value) {
        this.pagesize = value;
      },
      handleCurrentChange (value) {
        this.currentpage = value;
      },
      addRowClass ({row, rowIndex}) {
      if (row.rateType === NEGATIVE) {
        return 'warning-row';
      }
  }
},
    filters: {
      rateTypeToText (rateType) {
        return rateType === POSITIVE ? '已缴费' : '未缴费';
      },
      formatDate (time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm:ss');
      }
    },
    created () {
      this.Mes_Show();
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


