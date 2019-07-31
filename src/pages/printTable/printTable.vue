<template>
  <div class="main">
    <!-- <div class="table"> -->
     <table border="1" width="650" height="680" align="center" cellspacing="0">
		<caption color:black><strong>电子科技大学中山学院2019年新生报道凭条</strong></caption>
		<tr align="center">
		<td bgcolor="#ededed">考生号</td>
			<td colspan="2">{{this.$route.query.num}}</td>
			<td bgcolor="#ededed">新生姓名</td>
			<td colspan="2">{{this.$route.query.name}}</td>
		</tr>
		<tr align="center">
			<td bgcolor="#ededed">所属学院</td>
			<td colspan="2">{{this.$route.query.xueyuan}}</td>
			<td bgcolor="#ededed">录取专业</td>
			<td colspan="2">{{this.$route.query.zy}}</td>
		</tr>
		<tr align="center">
			<td bgcolor="#ededed">所属班级</td>
			<td colspan="7"></td>
		</tr>
		<tr align="center">
			<td bgcolor="#ededed">身份验证及缴费情况确定</td>
			<td colspan="7">{{this.$route.query.payment}}</td>
		</tr>
		<tr align="center">
			<td bgcolor="#ededed">交报道资料</td>
			<td colspan="7"></td>
		</tr>
		<tr align="center">
			<td bgcolor="#ededed">领取资料袋</td>
			<td colspan="7"></td>
		</tr>
    <tr align="center">
			<td bgcolor="#ededed">领取校园卡</td>
			<td colspan="7"></td>
		</tr>
    <tr align="center">
			<td bgcolor="#ededed">交档案</td>
			<td colspan="7"></td>
		</tr>
    <tr align="center">
			<td bgcolor="#ededed">办理户口迁移(需迁移户口者)</td>
			<td colspan="7"></td>
		</tr>
    <tr align="center">
			<!-- <td bgcolor="#ededed">办理户口迁移(需迁移户口者)</td> -->
			<td colspan="7">宿舍:入住<u style="top: -50px;">{{this.$route.query.dorm}}</u>栋<u>610</u>号宿舍<u>4</u>床</td>
		</tr>
	</table>
      <hr style="border:1 dashed #987cb9" width="100%  padding-bottom:5px;" color=black SIZE=1>
      <p  align='center'><strong>新生住宿凭条</strong></p>
  <p class="div2">
    <b class="f3">
        电子信息学院&nbsp;<u>{{this.$route.query.zy}}</u>专业&nbsp;<u>17电信B</u>班级<u>{{this.$route.query.name}}</u>同学入住&nbsp;<u>{{this.$route.query.dorm}}</u>栋<u>610</u>号宿舍<u>4</u>床。管理员凭条发放钥匙：新生入住后签《入住协议》。办理住宿卡（交寸照一张）。
  </b>
</p>
<p class="div4">
    <b>二级学院(盖章)</b>
</p>
<p class="div5">
  <b>经办人签名:</b> 
</p>
<p class="div5">
  <b>{{systemDate}}</b> 
</p>
    <!-- </div> -->
  </div>
</template>

<script>
export default {
  name: "printTable",
  data () {
    return {
      systemDate:""
    }
  },
  mounted () {
    // 获取缓存数据
    // this.province =this.$route.params.province
    window.print(); 
  },
  methods:{
    getTime(){
				// setInterval(()=>{
				// 	//new Date() new一个data对象，当前日期和时间
				// 	//toLocaleString() 方法可根据本地时间把 Date 对象转换为字符串，并返回结果。
				// 	this.nowtime = new Date().toLocaleString();
        // },1000)
         let nowDate = new Date();
         let date = {
                        year: nowDate.getFullYear(),
                        month: nowDate.getMonth() + 1,
                        date: nowDate.getDate(),
                    }
                    this.systemDate = date.year + '-' + 0 + date.month + '-' + date.date;
                    
                    
			}
    
  },
  created(){
			this.getTime();
		}
};
</script>

<style >
td{
		width: 80px;
	}
        .f3 {
            font-family: serif;
            
        }
        .div2{
            padding-left: 7%;
            padding-top:  1%;
        }
        .div4{
            padding-left: 65%;
            padding-top: 40px;
        }
        .div5{
            padding-left: 66%;
        }
        .div6 {
    width: 300px;
    border-bottom: 1px solid black;
}



  
</style>
