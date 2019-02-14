<template>
    <div class="container">
        <el-row>
            <el-alert
              title="第三方产品统计（UV统计是指00:00-24:00内相同的客户端只被计算一次，用户注册人次指的是只要用户注册就统计一次）"
              :closable="false"
              type="info">
            </el-alert>           
        </el-row>  
        <el-row class="m20 col-flex-end"  >

       
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
        </el-row>    
        <el-table
            :data="tableData"  
            border  
            ref="multipleTable" 
            tooltip-effect="dark"
                highlight-current-row style="width: 100%;font-weight:bold"
            class="m20"
            v-loading="loading"
          >
            <el-table-column prop="produectName" fixed label="产品名称" align="center"   min-width="170"></el-table-column>
            <el-table-column prop="registerDeviceVOtotal" label="UV统计数量" align="center"  min-width="70"></el-table-column>
            <el-table-column prop="registerVOtotal" label="用户注册人次" align="center" min-width="100"></el-table-column>
            <el-table-column prop="cz"  align="center" label="操作"   min-width="100">
                <template slot-scope="scope">
                <el-button
                    size="mini"
                    type="success"
                    @click="handleAllocation(scope.$index, scope.row)"
                   >UV统计</el-button>
                <el-button
                    size="mini"
                    type="success"
                    @click="handleRegister(scope.$index, scope.row)"
                   >注册量</el-button>                   
                </template> 
            </el-table-column> 
        </el-table>  
        <!-- <el-row class="m20" v-if="total>0">
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
        </el-row>                -->
        <el-dialog
          title="UV统计详情"
          :visible.sync="dialogUVVisible" 
          center
          width="80%"
          >
            <el-table
                :data="tableData1"  
                border  
                ref="multipleTable" 
                tooltip-effect="dark"
                highlight-current-row style="width: 100%;font-weight:bold"
                class="m20"
                v-loading="loading1"
              >
                <el-table-column prop="userId" fixed label="用户ID" align="center"   min-width="100"></el-table-column>
                <el-table-column prop="userName" label="用户姓名" align="center"  min-width="120"></el-table-column>
                <el-table-column prop="deviceId" label="设备号" align="center" min-width="100"></el-table-column>
                <el-table-column prop="registerTime" label="注册时间" align="center" min-width="170">
                <template slot-scope="scope">
                    {{scope.row.registerTime|dateServer}}
                </template>                    
                </el-table-column>
            </el-table> 
            <el-row class="m20" v-if="total1>0">
                 <div style="float:right">
                     <el-pagination
                       @current-change="handleCurrentChange1"
                        @size-change="handleSizeChange1"
                       :current-page="npage1"
                        :page-sizes="[10, 20, 50, 100, 1000,990000]"
                         :page-size="pagesize1"
                       background
                       layout="total,sizes,prev, pager, next,jumper"
                       :total="total1">
                     </el-pagination>   
                 </div>
            </el-row>                         
        </el-dialog> 
        <el-dialog
          title="注册详情"
          :visible.sync="dialogREVisible" 
          center
          width="80%"
          >
            <el-table
                :data="tableData2"  
                border  
                ref="multipleTable" 
                tooltip-effect="dark"
                highlight-current-row style="width: 100%;font-weight:bold"
                class="m20"
                v-loading="loading2"
              >
                <el-table-column prop="userId" fixed label="用户ID" align="center"   min-width="100"></el-table-column>
                <el-table-column prop="userName" label="用户姓名" align="center"  min-width="120"></el-table-column>
                <el-table-column prop="registerVOtotal" label="注册时间" align="center" min-width="170">
                <template slot-scope="scope">
                    {{scope.row.registerTime|dateServer}}
                </template>                       
                </el-table-column>
            </el-table> 
            <el-row class="m20" v-if="total1>0">
                 <div style="float:right">
                     <el-pagination
                       @current-change="handleCurrentChange2"
                        @size-change="handleSizeChange2"
                       :current-page="npage2"
                        :page-sizes="[10, 20, 50, 100, 1000,990000]"
                         :page-size="pagesize2"
                       background
                       layout="total,sizes,prev, pager, next,jumper"
                       :total="total2">
                     </el-pagination>   
                 </div>
            </el-row>                         
        </el-dialog>                
    </div>
</template>

<script>
import { httpProductTotal, httpPublicDetails } from "../../../service/http";
import { timeFormat } from "../../../config/time";
export default {
  data() {
    return {
      search: {},
      tableData: [],
      loading: true,
      dialogUVVisible: false,
      dialogREVisible: false,
      npage: 1,
      pagesize: 10,
      total: 0,
      tableData1: [],
      npage1: 1,
      pagesize1: 10,
      total1: 0,
      loading1: true,
      tableData2: [],
      npage2: 1,
      pagesize2: 10,
      total2: 0,
      loading2: true,
      id1: null,
      id2: null
    };
  },
  methods: {
    getData(startTime, endTime) {
      httpProductTotal(startTime, endTime).then(res => {
        let data = res.data;
        if (data.code == 200) {
          this.tableData = data.data;
          this.loading = false;
        }
      });
    },
    getData1(produectId, pageNum, pageSize, type = 1) {
      httpPublicDetails(produectId, pageNum, pageSize, type).then(res => {
        let data = res.data;
        if (data.code == 200) {
          this.tableData1 = data.data.message;
          this.loading1 = false;

          this.total1 = data.data.total;
        }
      });
    },
    getData2(produectId, pageNum, pageSize, type = 2) {
      httpPublicDetails(produectId, pageNum, pageSize, type).then(res => {
        let data = res.data;
        if (data.code == 200) {
          this.tableData2 = data.data.message;
          this.loading2 = false;
          this.total2 = data.data.total;
        }
      });
    },
    reset() {
      this.search = {};
      this.getData();
    },
    handleSearch(type = true) {
      if (this.search.time && this.search.time.length) {
        this.getData(
          this.search.time[0] + " 00:00:00",
          timeFormat(this.search.time[1], 1) + " 00:00:00"
        );
      } else {
        this.getData();
      }
    },
    handleSearch1(type = true) {
      this.getData1(this.id1, this.pagesize1, this.npage1);
    },
    handleSearch2(type = true) {
      this.getData2(this.id2, this.pagesize2, this.npage2);
    },
    handleCurrentChange1(val) {
      this.npage1 = val;
      this.handleSearch1(false);
    },
    handleSizeChange1(val) {
      this.pagesize1 = val;
      this.handleSearch1(false);
    },
    handleCurrentChange2(val) {
      this.npage2 = val;
      this.handleSearch2(false);
    },
    handleSizeChange2(val) {
      this.pagesize2 = val;
      this.handleSearch2(false);
    },
    handleAllocation(index, row) {
      this.dialogUVVisible = true;
      this.npage1 = 1;
      this.pagesize1 = 10;
      this.id1 = row.id;
      this.getData1(this.id1, this.pagesize1, this.npage1);
    },
    handleRegister(index, row) {
      this.dialogREVisible = true;
      this.npage2 = 1;
      this.pagesize2 = 10;
      this.id2 = row.id;
      this.getData2(this.id2, this.pagesize2, this.npage2);
    }
  },
  mounted() {
    this.getData();
  }
};
</script>

<style>
</style>

