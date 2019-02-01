<template>
    <div class="container">
        <el-row>
            <el-alert
              :title="title"
              :closable="false"
              type="info">
            </el-alert>           
        </el-row>  
        <!-- <el-row class="m20" >
            <el-col   class="col-flex-end">
                    <el-button  type="primary" @click="reset">重置</el-button>
                    <el-date-picker
                    style="width:340px"
                    class="l20"
                      v-model="search.time"
                      type="daterange"
                      value-format="yyyy-MM-dd"
                      range-separator="至"
                      start-placeholder="开始日期"
                      end-placeholder="结束日期">
                    </el-date-picker>                
                    <el-button @click="handleSearch" class="l20" style="margin-left:20px" icon="el-icon-search"  type="success" circle></el-button>                                                                  
            </el-col>             
        </el-row>         -->
        <el-row style="margin-top:40px">
	        <div id="myChart" :style="{width: '1200px', height: '600px'}"></div>
        </el-row>   
    </div>
</template>
<script>
import { httpCollectremain } from "../../../service/http";
import { timeFormat } from "../../../config/time";
export default {
  data() {
    return {
      title: "催收效果统计",
      search: {},
      showVisible: true,
      showText: false
    };
  },
  methods: {
    getData() {
      httpCollectremain()
        .then(res => {
          let data = res.data;
          if (data.code == 200) {
            let xData = data.data.map(v => v.name);
            let dataO = data.data.map(v => v.collectingBenjinMoney);
            let dataT = data.data.map(v => v.collectingNumber);
            this.drawLine(xData, dataO, dataT);
          }
        })
        .catch(err => {
          let data = {
            code: 200,
            msg: "提交成功",
            data: [
              {
                id: null,
                name: "csjl",
                allOverdueMoney: 0,
                all_late_fee: 0,
                allActureRepayMoney: 100,
                allActureRepayLateFee: 0,
                collectingNumber: 7,
                collectingBenjinMoney: 6200,
                overdueMoney_m1: 0,
                late_free_m1: 0,
                collect_money_m1: 0,
                overdueMoney_m2: 0,
                late_free_m2: 0,
                collect_money_m2: 0,
                overdueMoney_m3: 0,
                late_free_m3: 0,
                collect_money_m3: 0,
                overdueMoney_m4: 0,
                late_free_m4: 0,
                collect_money_m4: 0
              },
              {
                id: null,
                name: "潘月",
                allOverdueMoney: 0,
                all_late_fee: 0,
                allActureRepayMoney: 200,
                allActureRepayLateFee: 0,
                collectingNumber: 1,
                collectingBenjinMoney: 1500,
                overdueMoney_m1: 0,
                late_free_m1: 0,
                collect_money_m1: 0,
                overdueMoney_m2: 0,
                late_free_m2: 0,
                collect_money_m2: 0,
                overdueMoney_m3: 0,
                late_free_m3: 0,
                collect_money_m3: 0,
                overdueMoney_m4: 0,
                late_free_m4: 0,
                collect_money_m4: 0
              },
              {
                id: null,
                name: "王亦婷",
                allOverdueMoney: 0,
                all_late_fee: 0,
                allActureRepayMoney: 0,
                allActureRepayLateFee: 0,
                collectingNumber: 13,
                collectingBenjinMoney: 13300,
                overdueMoney_m1: 0,
                late_free_m1: 0,
                collect_money_m1: 0,
                overdueMoney_m2: 0,
                late_free_m2: 0,
                collect_money_m2: 0,
                overdueMoney_m3: 0,
                late_free_m3: 0,
                collect_money_m3: 0,
                overdueMoney_m4: 0,
                late_free_m4: 0,
                collect_money_m4: 0
              },
              {
                id: null,
                name: "赵丹丹",
                allOverdueMoney: 0,
                all_late_fee: 0,
                allActureRepayMoney: 0,
                allActureRepayLateFee: 0,
                collectingNumber: 12,
                collectingBenjinMoney: 11000,
                overdueMoney_m1: 0,
                late_free_m1: 0,
                collect_money_m1: 0,
                overdueMoney_m2: 0,
                late_free_m2: 0,
                collect_money_m2: 0,
                overdueMoney_m3: 0,
                late_free_m3: 0,
                collect_money_m3: 0,
                overdueMoney_m4: 0,
                late_free_m4: 0,
                collect_money_m4: 0
              },
              {
                id: null,
                name: "陈绍群",
                allOverdueMoney: 0,
                all_late_fee: 0,
                allActureRepayMoney: 0,
                allActureRepayLateFee: 0,
                collectingNumber: 10,
                collectingBenjinMoney: 9100,
                overdueMoney_m1: 0,
                late_free_m1: 0,
                collect_money_m1: 0,
                overdueMoney_m2: 0,
                late_free_m2: 0,
                collect_money_m2: 0,
                overdueMoney_m3: 0,
                late_free_m3: 0,
                collect_money_m3: 0,
                overdueMoney_m4: 0,
                late_free_m4: 0,
                collect_money_m4: 0
              },
              {
                id: null,
                name: "梁华金",
                allOverdueMoney: 0,
                all_late_fee: 0,
                allActureRepayMoney: 0,
                allActureRepayLateFee: 0,
                collectingNumber: 8,
                collectingBenjinMoney: 7300,
                overdueMoney_m1: 0,
                late_free_m1: 0,
                collect_money_m1: 0,
                overdueMoney_m2: 0,
                late_free_m2: 0,
                collect_money_m2: 0,
                overdueMoney_m3: 0,
                late_free_m3: 0,
                collect_money_m3: 0,
                overdueMoney_m4: 0,
                late_free_m4: 0,
                collect_money_m4: 0
              },
              {
                id: null,
                name: "刘婧",
                allOverdueMoney: 0,
                all_late_fee: 0,
                allActureRepayMoney: 0,
                allActureRepayLateFee: 0,
                collectingNumber: 16,
                collectingBenjinMoney: 16600,
                overdueMoney_m1: 0,
                late_free_m1: 0,
                collect_money_m1: 0,
                overdueMoney_m2: 0,
                late_free_m2: 0,
                collect_money_m2: 0,
                overdueMoney_m3: 0,
                late_free_m3: 0,
                collect_money_m3: 0,
                overdueMoney_m4: 0,
                late_free_m4: 0,
                collect_money_m4: 0
              }
            ]
          };
          if (data.code == 200) {
            let xData = data.data.map(v => v.name);
            let dataO = data.data.map(v => v.collectingBenjinMoney);
            let dataT = data.data.map(v => v.collectingNumber);
            this.drawLine(xData, dataO, dataT);
          }
        });
    },
    drawLine(xData, dataO, dataT) {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("myChart"));
      var labelOption = {
        normal: {
          formatter: "{c}  {name|{a}}",
          fontSize: 16,
          rich: {
            name: {
              textBorderColor: "#fff"
            }
          }
        }
      };
      // 绘制图表
      myChart.setOption({
        title: { text: "催收余量统计" },
        color: ["#95CDFE", "#424247"],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow"
          }
        },
        legend: {
          data: ["订单金额", "余量笔数"]
        },
        calculable: true,
        xAxis: [
          {
            type: "category",
            axisTick: { show: false },
            data: xData
          }
        ],
        yAxis: [
          {
            type: "value"
          }
        ],
        yAxis: [
          {
            type: "value",
            axisLabel: {
              formatter: "{value} "
            },
            axisLabel: {
              show: true,
              textStyle: {
                color: "#66c2e0"
              }
            },
            name: "(订单金额)",
            nameTextStyle: {
              color: "#66c2e0"
            }
          },
          {
            type: "value",
            axisLabel: {
              formatter: "{value} "
            },
            axisLabel: {
              show: true,
              textStyle: {
                color: "#66c2e0"
              }
            },
            name: "(余量笔数)",
            nameTextStyle: {
              color: "#66c2e0"
            }
          }
        ],
        series: [
          {
            name: "订单金额",
            type: "bar",
            barGap: 0,
            label: labelOption,
            data: dataO
          },
          {
            name: "余量笔数",
            yAxisIndex: 1,
            type: "bar",
            label: labelOption,
            data: dataT
          }
        ]
      });
    }
  },
  mounted() {
    this.getData();
  }
};
</script>
<style scoped>
</style>

<style scoped>
.title {
  padding: 10px;
  font-weight: bold;
  font-size: 18px;
  width: 100%;
}
.input-width {
  width: 215px;
}
.flx {
  position: absolute;
  width: 85%;
  top: 0px;
  padding-top: 10px;
  overflow: hidden;
  line-height: 30px;
}
.fl {
  float: left;

  font-size: 24px;
  font-weight: bold;
}
.fl span {
  font-size: 12px;
  font-weight: normal;
}
.fr {
  float: right;
  font-size: 12px;
}
.bar {
  position: relative;
  width: 50%;
  display: inline-block;
}
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
</style>
