<template>
    <div class="container">
        <el-dialog  width="60%" :visible.sync="s" style="position:relative;">
              
                  <el-row id="bg" style="width:100%;height:100%;position:absolute;top:0;left:0;zIndex:9999" :style ="note" >
                    <div >

                    </div>
                  </el-row>
                
            <el-row class="xy_flex">
                <div >
                    新颜报告
                </div>
                <div style="flex-grow:1">
                </div>
                <div >
                    报告时间:<span>{{new Date() |dateServer}}</span>
                </div>
            </el-row>   
            <hr>
             <div  v-if="!reportList" class="xy_title" style="margin-top:20px;">暂无报告</div>
            <div class="xy_title" style="margin-top:20px;">新颜报告</div>
            <el-row >
                <el-card >
                    <div class="xy_card">
                        <div>
                            <dl>
                                <dt>客户基本资料</dt>
                                <dd>姓名：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>陈宏波</span></dd>
                                <dd>手机号：&nbsp;&nbsp;&nbsp;<span>15366512811</span></dd>
                                <dd>身份证号：<span>320981199210294290</span></dd>
                            </dl>
                        </div>
                        <div>
                                <div class="progress-text">
                                    <strong>{{reportList.loans_score}}</strong>
                                    <p>贷款行为分</p>
                                </div>
                                <el-progress type="circle" :percentage="reportList.loans_score/10" color="red" :show-text="showText">aaaaa</el-progress>                        
                                <div class="xy_zxd">贷款行为置信度：{{reportList.loans_credibility}}</div>
                        </div>
                        <img class="xy_img" :src="reportList.loans_score > 500? require('../../../assets/img/u236.png'): require('../../../assets/img/u115.png')" alt="">
                    </div>


                    <!-- <el-col :span="6" :offset="2">
                        
                        <div >
                            <div class="progress-text">
                                <strong>{{reportList.loans_score}}</strong>
                                <p>贷款行为分</p>
                            </div>
                                <el-progress type="circle" :percentage="reportList.loans_score" color="red" :show-text="showText">aaaaa</el-progress>
                        </div>
                    </el-col>
                    <el-col :span="12">
                        <el-row style="text-align:center;padding-top:30px">
                           <strong style="font-size:20px">
                               贷款行为置信度<b style="margin-left:10px;color:red">{{reportList.loans_credibility}}</b>
                              </strong>
                        <br/>
                        </el-row>
                    </el-col> -->
                </el-card>
            </el-row> 
              <div class="xy_title" style="margin-top:20px;">规则命中详情</div>
              <el-card>
                <table class="xy_table">
                    <tr>
                        <th style="width:40%">命中规则详情</th>
                        <th style="width:60%">取值结果</th>
                    </tr>
                    <tr>
                        <td>总贷款放款订单数(12个月内):</td>
                        <td>{{reportList.loans_count}}</td>
                    </tr>
                    <tr>
                        <td>已结清贷款订单数(12个月内):</td>
                        <td>{{reportList.loans_settle_count}}</td>
                    </tr>
                    <tr>
                        <td>逾期贷款（M0+）订单数(12个月内):</td>
                        <td>{{reportList.loans_overdue_count}}</td>
                    </tr>
                    <tr>
                        <td>贷款放款机构数(12个月内):</td>
                        <td>{{reportList.loans_org_count}}</td>
                    </tr>  
                    <tr>
                        <td>贷款放款的消费金融类机构数(12个月内):</td>
                        <td>{{reportList.consfin_org_count}}</td>
                    </tr>  
                    <tr>
                        <td>查询主体贷款放款的网络贷款类机构数(12个月内):</td>
                        <td>{{reportList.loans_cash_count}}</td>
                    </tr>  
                    <tr>
                        <td> 近1个月的贷款放款笔数:</td>
                        <td>{{reportList.latest_one_month}}</td>
                    </tr> 
                    <tr>
                        <td> 近3个月的贷款放款笔数:</td>
                        <td>{{reportList.latest_three_month }}</td>
                    </tr> 
                    <tr>
                        <td> 近6个月的贷款放款笔数:</td>
                        <td>{{reportList.latest_six_month}}</td>
                    </tr> 
                    <tr>
                        <td> 贷款机构历史成功扣款笔数(12个月内):</td>
                        <td>{{reportList.history_suc_fee}}</td>
                    </tr> 
                    <tr>
                        <td> 贷款机构历史失败扣款笔数(12个月内):</td>
                        <td>{{reportList.history_fail_fee}}</td>
                    </tr> 
                    <tr>
                        <td>  近一个月的贷款机构成功扣款笔数:</td>
                        <td>{{reportList.latest_one_month_suc}}</td>
                    </tr> 
                    <tr>
                        <td>近一个月的贷款机构失败扣款笔数:</td>
                        <td>{{reportList.latest_one_month_fail}}</td>
                    </tr> 
                    <tr>
                        <td>  第一次贷款放款记录至今的天数:</td>
                        <td>{{reportList.loans_long_time}}</td>
                    </tr> 
                    <tr>
                        <td>  最后一次贷款放款记录时间(12个月内):</td>
                        <td>{{reportList.loans_latest_time}}</td>
                    </tr>                                                                                                                                                                                                                                                                                  
                </table>
              </el-card>
        </el-dialog> 
        <canvas id="myCanvas" width="200" height="230" >
        </canvas>
    </div>
</template>

<script>
export default {
  data() {
    return {
      s: true,
      reportList: {
        loans_count: "12",
        loans_long_time: "355",
        consfin_org_count: "2",
        latest_six_month: "2",
        loans_cash_count: "6",
        history_fail_fee: "16",
        latest_three_month: "0",
        latest_one_month_fail: "0",
        latest_one_month: "0",
        latest_one_month_suc: "0",
        loans_latest_time: "2018-07-26",
        history_suc_fee: "7",
        loans_org_count: "8",
        loans_credibility: "72",
        loans_score: "581",
        loans_overdue_count: "3",
        loans_settle_count: "6"
      },
      showText: false,
      canvasShow: true,
      note: {
        backgroundImage: ""
        // backgroundRepeat: "no-repeat",
        // backgroundSize: "25px auto",
        // marginTop: "5px"
      }
    };
  },
  mounted() {
    setTimeout(() => {
      if (this.canvasShow) {
        var canvas = document.getElementById("myCanvas");
        var ctx = canvas.getContext("2d");
        ctx.save();
        ctx.font = "30px Microsoft YaHei";
        ctx.rotate(-45 * Math.PI / 180);
        ctx.fillStyle = "rgba(0, 0, 0, 0.1)";
        ctx.fillText("恒盾云", -100, 220);
        ctx.restore();
        this.note.backgroundImage = 'url("' + ctx.canvas.toDataURL() + '")';
        console.log(this.note.backgroundImage);
        this.canvasShow = false;
      }
    }, 20);
  }
};
</script>
<style scoped>
.progress-text {
  position: absolute;
  text-align: center;
  font-size: 32px;
  line-height: 126px;
  width: 126px;
  height: 126px;
}
.progress-text p {
  width: 100%;
  position: absolute;
  top: 80px;
  font-size: 12px;
  line-height: 20px;
  text-align: center;
}
.table {
  border-spacing: 2px;
  border-color: grey;
  font-size: 13px;
  padding-left: 20px;
  width: 100%;
  display: table;
  border-collapse: collapse;
}
thead tr {
  border-bottom: 2px solid #eaeaea;
}
th {
  padding-left: 10px;
  height: 36px;
}
.borderBottom1 {
  border-bottom: 1px solid #eaeaea;
}
.borderBottom2 {
  border-bottom: 1px solid #eaeaea;
}
.risk-detail-list {
  margin-left: 40px;
}
ul,
li {
  list-style: none;
}
.check-table {
  width: 100%;
}
.xy_flex {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  font-weight: bold;
  font-size: 14px;
  line-height: 28px;
}
.xy_title {
  text-align: center;
  font-weight: bold;
  font-size: 24px;
  line-height: 64px;
  background: #409eff;
  color: #fff;
  border-radius: 5px 5px 0 0;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}
.xy_card {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  position: relative;
  align-items: center;
}
.xy_card:first-child {
}
.xy_zxd {
  font-weight: bold;
  font-size: 20px;
  line-height: 36px;
  margin-top: 20px;
  position: relative;
  left: -20px;
}
.xy_img {
}
.xy_card dt {
  border-left: 4px solid #409eff;
  padding-left: 10px;
  margin-bottom: 20px;
}
.xy_card dd {
  font-size: 16px;
  line-height: 32px;
}
.xy_table {
  width: 100%;
  border: 1px solid #cccccc;
  border-collapse: collapse;
  text-align: left;
}
.xy_table th {
  background: #f1f1f1;
}
.xy_table th,
td {
  border: 1px solid #cccccc;
  line-height: 36px;
  padding-left: 30px;
}
</style>

