<template>
    <div class="container">
        <el-row>
            <el-alert
              title="推广情况统计"
              :closable="false"
              type="info">
            </el-alert>           
        </el-row>
        <el-row class="m20">

            <el-col  :span="14" :offset="10" class="col-flex-end">
                <el-button  type="primary" @click="reset">重置</el-button>
                    <el-select  v-model="search.role" placeholder="选择推广码" class="l20">
                      <el-option
                        v-for="(item,i) in codes"
                        :key="i"
                        :label="item"
                        :value="item">
                      </el-option>
                    </el-select>                 
                    <el-date-picker
                      class="l20"
                         style="width:340px"
                      v-model="search.time"
                      type="daterange"
                      value-format="yyyy-MM-dd"
                      range-separator="至"
                      start-placeholder="开始日期"
                      end-placeholder="结束日期">
                    </el-date-picker>       
                    <!-- <div class="l20">
                        <el-input
                        style="padding:0px 10px 0px 0px"
                          placeholder="请输入关键词"
                          v-model="search.input"
                          clearable>
                        </el-input> 
                    </div>                                 -->
                    <el-button class="l20" style="margin-left:10px" @click="handleSearch" icon="el-icon-search"  type="success" circle></el-button>                     
            </el-col>
        </el-row>
        <el-row class="m20">
            <el-table
                    :data="tableData"
                    border
                    v-loading="loading"
                        highlight-current-row style="width: 100%;font-weight:bold">
                <el-table-column prop="id" fixed  align="center" label="渠道id"  width="70" ></el-table-column>
                <el-table-column prop="createTime" label="创建时间" align="center" min-width="140"  >
                    <template slot-scope="scope">
                        {{scope.row.createTime|dateServer}}
                    </template>
                </el-table-column>       
  
                <el-table-column prop="promotionCode" align="center" label="推广码" min-width="90" ></el-table-column>                                    
                <el-table-column prop="spje" align="center"  label="推广二维码"  min-width="90">
                    <template slot-scope="scope">
                      <div @click="handleImg(scope.$index, scope.row)" style="cursor:pointer">
                      		<!-- <vue-qr   :logoSrc="scope.row.chbqrcode.imagePath" :text="scope.row.chbqrcode.value" :size="20" :margin="0"></vue-qr> -->
                          <vue-qr :bgSrc='config.imagePath' :logoSrc="config.imagePath" :text="config.value" :size="20" :margin="0"></vue-qr>
                      </div>
                    </template>
                </el-table-column>
                <el-table-column prop="overdueRate_m1" align="center" label="m1逾期订单率"     min-width="100" >
                    <template slot-scope="scope">
                    <span>{{(scope.row.overdueRate_m1.toFixed(4)*100).toFixed(2)}}%</span>
                    </template>                  
                </el-table-column>  
                <el-table-column prop="overdueRate_m2" align="center" label="m2逾期订单率"     min-width="100" >
                    <template slot-scope="scope">
                    <span>{{(scope.row.overdueRate_m2.toFixed(4)*100).toFixed(2)}}%</span>
                    </template>                   
                </el-table-column>  
                <el-table-column prop="overdueRate_m3" align="center" label="m3逾期订单率"     min-width="100" >
                    <template slot-scope="scope">
                    <span>{{(scope.row.overdueRate_m3.toFixed(4)*100).toFixed(2)}}%</span>
                    </template>                   
                </el-table-column>  
                <el-table-column prop="overdueRate_m4" align="center" label="m3+逾期订单率"     min-width="110" >
                    <template slot-scope="scope">
                    <span>{{(scope.row.overdueRate_m4.toFixed(4)*100).toFixed(2)}}%</span>
                    </template>                   
                </el-table-column>                                                                  
                <el-table-column prop="promotionMoblie" align="center" label="推广人账号"     min-width="100" >
                 
                </el-table-column>    
                <el-table-column prop="registeredSum"  align="center" label="注册数量"  min-width="80" >
                 <template slot-scope="scope">
                        <el-button
                        size="mini"
                        type="success"
                        @click="export2Excel(scope.$index, scope.row,'registeredSum')"
                        >{{scope.row.registeredSum}}</el-button>  
                    </template>                    
                </el-table-column>
                <el-table-column prop="applySum"  align="center" label="申请数量"   min-width="80">
                 <template slot-scope="scope">
                        <el-button
                        size="mini"
                        type="success"
                        @click="export2Excel(scope.$index, scope.row,'applySum')"
                        >{{scope.row.applySum}}</el-button>  
                    </template>                   
                  </el-table-column>      
                <el-table-column prop="standardApplyPassSum"  align="center" label="期间注册并提交机审"   min-width="130">
                                  <template slot-scope="scope">
                        <el-button
                        size="mini"
                        type="success"
                        @click="export2Excel(scope.$index, scope.row,'standardApplyPassSum')"
                        >{{scope.row.standardApplyPassSum}}</el-button>  
                    </template> 
                  </el-table-column>                                             
                <el-table-column prop="applyPassSum" align="center" label="机审通过数量" min-width="90" >
                 <template slot-scope="scope">
                        <el-button
                        size="mini"
                        type="success"
                        @click="export2Excel(scope.$index, scope.row,'applyPassSum')"
                        >{{scope.row.applyPassSum}}</el-button>  
                    </template>                   
                  </el-table-column>            
                    <el-table-column prop="applyPassSumByBeauty" align="center" label="已开通" min-width="90" >
                 <template slot-scope="scope">
                        <el-button
                        size="mini"
                        type="success"
                        @click="export2Excel(scope.$index, scope.row,'applyPassSumByBeauty')"
                        >{{scope.row.applyPassSumByBeauty}}</el-button>  
                    </template>                         
                    </el-table-column>
                 
                <el-table-column prop="firstWithdraw"  align="center" label="首单提现额"   min-width="90">
                <!-- <template slot-scope="scope">
                        <el-button
                        size="mini"
                        type="success"
                        @click="export2Excel(scope.$index, scope.row,'firstWithdraw')"
                        >{{scope.row.firstWithdraw}}</el-button>  
                    </template>                    -->
                                
                  </el-table-column>                   
                <el-table-column prop="allWithdraw"  align="center" label="总提现额度"  min-width="90" >
                <!-- <template slot-scope="scope">
                        <el-button
                        size="mini"
                        type="success"
                        @click="export2Excel(scope.$index, scope.row,'allWithdraw')"
                        >{{scope.row.allWithdraw}}</el-button>  
                    </template>                     -->
                  </el-table-column>                   
                
  
                <!-- <el-table-column prop="zt" align="center" label="查看提现" >
                    <template slot-scope="scope">
                        <el-button
                        size="mini"
                        type="success"
                        @click="handleShow"
                        >提现情况</el-button>  
                    </template>
                </el-table-column> -->
            </el-table>            
        </el-row>
        <el-row class="m20" v-if="total>0">
             <div style="float:right">
                 <el-pagination
                   @current-change="handleCurrentChange"
                    @size-change="handleSizeChange"
                   :current-page="npage"
                    :page-sizes="[10, 20, 50, 100, 1000,990000]"
                     :page-size="pagesize"
                   background
                   layout="total,sizes,prev, pager, next,jumper"
                   :total="total">
                 </el-pagination>   
             </div>
        </el-row> 
        <el-dialog 
                width="60%"
              title="查看提现情况"
              center
              :visible.sync="showVisible"   
            >
                <el-table
                    :data="lateFeesData"
                    border
                        highlight-current-row style="width: 100%;font-weight:bold">  
                    <el-table-column prop="sqr" align="center" label="样式一" ></el-table-column>                    
                    <el-table-column prop="sj"  align="center" label="样式二" ></el-table-column>                    
                    <el-table-column prop="wyje" align="center" label="样式三" ></el-table-column>                    
                </el-table>                  
        </el-dialog> 
        <el-dialog 
                width="30%"
              title="二维码展示"
              center
              :visible.sync="imgVisible"   
            >
              <el-row>

              </el-row>
              <el-tabs type="border-card">
                  <el-tab-pane label="样式一" style="text-align:center">
                      <vue-qr   :logoSrc="imgSrc.imagePath" :text="imgSrc.value" :size="200" :margin="0"></vue-qr>
                      <div >
                        <span>您的二维码</span>
                      </div>                    
                  </el-tab-pane>
                  <el-tab-pane label="样式二" style="text-align:center">
                     <vue-q-art :config="imgSrc"></vue-q-art>
                      <div >
                        <span>您的二维码</span>
                      </div>                    
                  </el-tab-pane>
                  <el-tab-pane label="样式三" style="text-align:center">
                  		<vue-qr :bgSrc='config.imagePath' :logoSrc="config.imagePath" :text="config.value" :size="200" :margin="0"></vue-qr>
                      <div >
                        <span>您的二维码</span>
                      </div>                    
                  </el-tab-pane>
              </el-tabs>
                    <!-- <el-card :body-style="{ padding: '20px 0px',textAlign:'center' }">
                    
                      <vue-qr   :logoSrc="imgSrc.imagePath" :text="imgSrc.value" :size="200" :margin="0"></vue-qr>
                      <div >
                        <span>您的二维码</span>
                      </div>
                    </el-card> -->
        </el-dialog>  
    </div>
</template>
<script>
import { mapGetters } from "vuex";
import { timeFormat } from "../../../config/time";
import VueQArt from "vue-qart";
import VueQr from "vue-qr";

import { getPromoterList, httpFindAllPrometion } from "../../../service/http";
export default {
  components: {
    VueQArt,
    VueQr
  },
  computed: {
    // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      "loginId",
      "key"
      // ...
    ])
  },
  data() {
    return {
      config: {
        value: "www.baidu.com", //显示的值、跳转的地址

        logo: "../../../assets/logo.png" //中间logo的地址
      },
      codes: [],
      src2: "../../../assets/logo.png",
      imgSrc: {},
      timeout: null,
      search: {
        time: [],
        input: "",
        role: "",
        roles: [
          {
            value: "0",
            label: "催收员"
          },
          {
            value: "1",
            label: "无催收员"
          },
          {
            value: "2",
            label: "潘月"
          },
          {
            value: "3",
            label: "王怡婷"
          }
        ],
        role: ""
      },
      tableData: [],
      total: 0,
      npage: 1,
      pagesize: 10,
      addVisible: false,
      showVisible: false,
      imgVisible: false,
      loading: false,
      addForm: {
        id: "",
        price: "",
        hasPrice: "",
        textarea: ""
      },
      restaurants: [],
      lateFeesData: [
        {
          sqr: "	借花花",
          sj: "2018-03-08",
          wyje: "1780.00",
          qs: "14",
          khm: "蒋成富",
          spmc: "钱包取现1000元",
          sqbz: "连连支付滞纳金"
        }
      ]
    };
  },
  watch: {
    // imgVisible(newV) {
    //   console.log(newV);
    //   if (newV) {
    //     this.imgSrc = null;
    //   }
    // }
  },
  methods: {
    //得到推广码
    _httpFindAllPrometion() {
      httpFindAllPrometion()
        .then(res => {
          let data = res.data;
          if (data.code == 200) {
            this.codes = data.data;
          }
        })
        .catch(err => {});
    },
    _config(url) {
      return {
        value: url,
        imagePath: require("../../../../static/img/favicon1.jpg"),
        filter: "color"
      };
    },
    _getPromoterList(
      npage,
      pagesize,
      loginId,
      startTime,
      endTime,
      PromotionCode
    ) {
      let _this = this;

      _this.loading = true;
      getPromoterList(
        npage,
        pagesize,
        loginId,
        startTime,
        endTime,
        PromotionCode
      )
        .then(res => {
          let data = res.data;
          let self = _this;
          _this.total = data.total;
          for (let a = 0; a < data.rows.length; a++) {
            console.log(_this);
            data.rows[a].chbqrcode = self._config(data.rows[a].qrcode);
          }
          _this.tableData = data.rows;
          _this.loading = false;
          console.log(_this.tableData);
        })
        .catch(err => {
          this.$message.error("网络错误请联系管理员");
        });
    },
    handleSearch(type = true) {
      if (type) {
        this.npage = 1;
        this.pagesize = 10;
      }
      if (this.search.time && this.search.time.length) {
        this._getPromoterList(
          this.npage,
          this.pagesize,
          this.loginId,
          this.search.time[0] + " 00:00:00",
          timeFormat(this.search.time[1], 1) + "00:00:00",
          this.search.role
        );
      } else {
        this._getPromoterList(
          this.npage,
          this.pagesize,
          this.loginId,
          "",
          "",
          this.search.role
        );
      }
    },
    handleCurrentChange(val) {
      this.npage = val;
      this.handleSearch(false);
    },
    handleSizeChange(val) {
      this.pagesize = val;
      this.handleSearch(false);
    },
    handleAdd() {
      this.addVisible = true;
    },
    createStateFilter(queryString) {
      return state => {
        return (
          state.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
        );
      };
    },
    handleSelect(item) {
      console.log(item);
    },
    loadAll() {},
    beforeAvatarUpload(file) {
      const isJPG = file.type === "image/jpeg";
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error("上传头像图片只能是 JPG 格式!");
      }
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 2MB!");
      }
      return isJPG && isLt2M;
    },
    addSubmit() {
      console.log("submit!");
    },
    handleShow() {
      this.showVisible = true;
    },
    handleImg(index, row) {
      this.imgSrc = null;
      if (row) {
        this.imgSrc = this._config(row.qrcode);
        console.log(this.imgSrc.imagePath);
        this.imgVisible = true;
      }
    },

    // qrcode() {
    //   let qrcode = new QRCode("qrcode", {
    //     width: 100,
    //     height: 100, // 高度
    //     text: "56663159" // 二维码内容
    //     // render: 'canvas' // 设置渲染方式（有两种方式 table和canvas，默认是canvas）
    //     // background: '#f0f'
    //     // foreground: '#ff0'
    //   });
    //   console.log(qrcode);
    // }
    //excel导出数据
    export2Excel(index, row, title) {
      if (this.key !== "myWorkbench") return;
      let t = title + "List";
      require.ensure([], () => {
        const {
          export_json_to_excel
        } = require("../../../vendor/Export2Excel");
        const tHeader = ["姓名", "手机号"];
        const filterVal = ["username", "moblie"];
        let list = JSON.parse(JSON.stringify(row[t]));

        // if (this.key !== "myWorkbench") {
        //   for (var i = 0; i < list.length; i++) {
        //     list[i].moblie = "";
        //     list[i].username = "";

        //   }
        // }
        const data = this.formatJson(filterVal, list);
        export_json_to_excel(tHeader, data, "推广情况统计");
      });
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => v[j]));
    },
    reset() {
      this.search = {};
      this.npage = 1;
      this.pagesize - 10;
      this.handleSearch();
    }
  },
  mounted() {
    setTimeout(() => {
      this._getPromoterList(this.npage, this.pagesize, this.loginId);
    }, 20);
    this._httpFindAllPrometion();
  }
};
</script>
<style scoped>
.col-flex-end {
  display: flex;
  justify-content: flex-end;
}
.image {
  width: 100%;
  display: block;
}
#query {
  width: 100px;
  height: 100px;
}
#canvas {
  width: 80% !important;
  height: auto !important;
}
</style>

