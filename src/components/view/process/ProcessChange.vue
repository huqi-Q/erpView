<template>
<div>
<!--产品工序设计-->
  <!--  条件查询-->
  <el-form ref="productFrom"  :inline="true">
    <el-form-item label="请选择产品I级分类">
      <el-select v-model="value" placeholder="请选择">
        <el-option
          :key="firstKindId"
          :label="firstKindName"
          :value="firstKindId">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="请选择产品II级分类">
      <el-select v-model="value" placeholder="请选择">
        <el-option
          :key="firstKindId"
          :label="firstKindName"
          :value="firstKindId">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="请选择产品III级分类">
      <el-select v-model="value" placeholder="请选择">
        <el-option
          :key="firstKindId"
          :label="firstKindName"
          :value="firstKindId">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="请输入登记时间">
      <div class="block">
        <el-date-picker
          v-model="registerTime"
          type="daterange"
          align="right"
          unlink-panels
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          :shortcuts="shortcuts"
        >
        </el-date-picker>
      </div>
    </el-form-item>

    <el-button type="success" @click="searchproduct">查询</el-button>
    <el-button type="danger" @click="resetproduct(productFrom)">重置</el-button>
  </el-form>


  <!--表格 -->
  <el-table
    :data="tableData"
    stripe
    border
    style="width: 100%">
    <el-table-column
      prop="productId"
      label="产品编号"
      width="180">
    </el-table-column>
    <el-table-column
      prop="productName"
      label="产品名称">
    </el-table-column>
    <el-table-column
      prop="productClass"
      label="档次级别">
    </el-table-column>
    <el-table-column
      prop="firstKindName"
      label="I级分类">
    </el-table-column>
    <el-table-column
      prop="secondKindName"
      label="II级分类">
    </el-table-column>
    <el-table-column
      prop="thirdKindName"
      label="III级分类">
    </el-table-column>
    <el-table-column
      prop="responsiblePerson"
      label="产品经理">
    </el-table-column>
    <el-table-column
      label="变更">
      <template slot-scope="scope">
        <a href="#" @click.prevent='bg(scope.row)'>变更</a>
      </template>
    </el-table-column>
  </el-table>
  <!-- 分页-->
  <el-pagination
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange"
    :current-page="pageno"
    :page-sizes="[5, 10, 15, 20]"
    :page-size="pagesize"
    layout="total, sizes, prev, pager, next, jumper"
    :total="total">
  </el-pagination>

<!--  设计单-->
  <el-dialog width="80%" title="生产工序设计单" :visible="zdwinshow">

    <el-form  :modal="scFrom">
      <el-row>
        <el-col :span="12">
          <div class="grid-content bg-purple">
          <strong style="margin-right: 220px">产  品  名  称  :  {{scFrom.productName}}</strong>
          <br>
          <br>
          <strong >设计人:</strong>
          <input v-model="scFrom.register" class="xhx" style="width:300px"></input>
          </div>
        </el-col>
        <el-col :span="12">
          <div class="grid-content bg-purple-light">
         <strong>产  品   编 号  :  {{scFrom.productId}}</strong>
          </div>
        </el-col>
          </el-row>
      <br>
          <!--生产工序-->
            <div>
              <el-table
                :data="scgxtableData"
                stripe
                border
                :cell-style="changeCellStyle"
                style="width: 100%">
                <el-table-column
                  type="selection"
                  width="55">
                </el-table-column>
                <el-table-column
                  prop="procedureName"
                  label="工序名称"
                  width="180">
                </el-table-column>
                <el-table-column
                  prop="procedureId"
                  label="工序编号">
                </el-table-column>
                <el-table-column
                  prop="procedureDescribe"
                  label="描述">
                </el-table-column>
                <el-table-column
                  prop="labourHourAmount"
                  label="工时数">
<!--                  <template slot-scope="scope">-->
<!--                    <input class="mbk" v-model="labourHourAmount" type="text"></input>-->
<!--                  </template>-->
                </el-table-column>
                <el-table-column
                  prop="amountUnit"
                  label="工时单位">
<!--                  <template slot-scope="scope">-->
<!--                    <input class="mbk" v-model="amountUnit" type="text"></input>-->
<!--                  </template>-->
                </el-table-column>
                <el-table-column
                  prop="costPrice"
                  label="单位工时成本">
<!--                  <template slot-scope="scope">-->
<!--                    <input class="mbk" v-model="costPrice" type="text"></input>-->
<!--                  </template>-->
                </el-table-column>
                <el-table-column
                  prop="subtotal"
                  label="工时成本小计(元)">
<!--                  <template slot-scope="scope">-->
<!--                    {{labourHourAmount*costPrice}}-->
<!--                  </template>-->
                </el-table-column>
              </el-table>
            </div>

      <el-row>
        <el-col :span="12"><div class="grid-content bg-purple">
          <strong >登记人:</strong>
          <input class="xhx" value="dd" disabled="disabled" style="width:300px"></input>
        </div></el-col>
        <el-col :span="12"><div class="grid-content bg-purple-light">
          <strong>登  记  时  间:  {{currentTime}}</strong>
        </div></el-col>
      </el-row>
      <br>
      <div style="margin-right:40px">
      <strong>设计要求</strong>
      <textarea style="width:950px;height: 100px">

      </textarea>
      </div>
      <br>

      <div>
        <el-button @click="tjgxtb">添加工序</el-button>
        <el-button @click="scgx">删除工序</el-button>
      </div>

      </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="qx">取 消</el-button>
                <el-button type="primary" @click="scgx">确 定</el-button>
              </div>
            </el-dialog>

<!--工序表格-->
  <el-dialog width="80%" title="生产工序设计单" :visible="gxwinshow">


      <!--生产工序-->
      <div>
        <el-table
          :data="manufactureData"
          stripe
          border
          style="width: 100%">
          <el-table-column
            prop="id"
            label="工序编号">
          </el-table-column>
          <el-table-column
            prop="procedureName"
            label="工序名称"
            width="180">
          </el-table-column>
          <el-table-column
            prop="procedureDescribe"
            label="工序描述">
          </el-table-column>
          <el-table-column
            label="添加">
            <template slot-scope="scope">
              <a href="#" @click.prevent='tjgx(scope.row)'>添加</a>
            </template>
          </el-table-column>
        </el-table>
      </div>

      <div>
        <el-button @click="gxwinshow = false">返回</el-button>
      </div>
  </el-dialog>

            </div>
          </template>

          <script>
              export default {
                  name: "ProcessChange",
                data() {
                  return {
                    tableData: [],
                    scgxtableData:[],
                    currentTime:new Date(),
                    zdwinshow: false,
                    gxwinshow:false,
                    scFrom:{},
                    labourHourAmount:0,
                    subtotal:0,
                    amountUnit:"",
                    costPrice:0,
                    manufactureData:[],
                    options:[],
                    designer:"",
                    value:"",
                    pageno: 1,
                    pagesize: 5,
                    total: 0,
                    firstKindId:"",
                    firstKindName:"",
                    shortcuts: [{
                      text: '最近一周',
                      value: (() => {
                        const end = new Date()
                        const start = new Date()
                        start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
                        return [start, end]
                      })(),
                    }, {
                      text: '最近一个月',
                      value: (() => {
                        const end = new Date()
                        const start = new Date()
                        start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
                        return [start, end]
                      })(),
                    }, {
                      text: '最近三个月',
                      value: (() => {
                        const end = new Date()
                        const start = new Date()
                        start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
                        return [start, end]
                      })(),
                    }],
                    registerTime: ''
                  }
                },
                methods: {
                  getdata() {   //获取数据
                    var _this = this;
                    var params = new URLSearchParams();
                    params.append("pageno", this.pageno);
                    params.append("pagesize", this.pagesize);
                    //产品档案
                    this.$axios.post("/mDesignProcedure/queryallProcedure2", params).then(function (response) {
                      _this.tableData = response.data.records;
                      _this.total = response.data.total;
                      // _this.firstKindName = response.data.records[0].firstKindName;
                      // _this.firstKindId = response.data.records[0].firstKindId;
                    }).catch();



                  },
                  changeCellStyle (row, column, rowIndex, columnIndex) {
                    if(row.column.label === "工时数"){
                      return 'background-color: blanchedalmond'  // 修改的样式
                    }else if(row.column.label === "工时单位"){
                      return 'background-color: blanchedalmond'
                    }else if(row.column.label === "单位工时成本"){
                      return 'background-color: blanchedalmond'
                    }
                    else{
                      return ''
                    }
                  },
                  tjgxtb(){
                    var _this = this;
                    this.gxwinshow = true;
                    this.$axios.post("/manufactureList/queryallmanufactureList.action").then(function (response) {
                      _this.manufactureData = response.data;
                    }).catch();
                  },
                  qx(){
                    this.zdwinshow = false;
                    this.scgxtableData=[];
                  },
                  handleSizeChange(val) {  //页size变更
                    this.pagesize = val;
                    this.pageno = 1;
                    this.getdata();
                  },
                  handleCurrentChange(val) {  //页码变更

                    this.pageno = val;
                    this.getdata();
                  },
                  searchproduct() {   //条件查询
                    this.getdata();
                  },
                  resetproduct(name){
                    this.$refs.productFrom.resetFields()
                    this.dialogVisible = false;
                  },
                  bg(row){
                    this.zdwinshow = true;
                    var _this = this;
                    var params = new URLSearchParams();
                    params.append("id",row.id);
                    this.$axios.post("/mDesignProcedure/selectById",params).then(function (response) {
                        _this.scFrom=response.data;
                    }).catch();
                    this.$axios.post("/mDesignProcedureDetails/selectById",params).then(function (response) {
                      _this.scgxtableData=response.data;
                    }).catch();
                  },
                  tjgx(row){
                      this.scgxtableData.push(row);
                  },
                  scgx(){
                    var _this = this;
                    this.$confirm('确定设计产品:  '+this.scFrom.productName+' ?', '提示', {
                      confirmButtonText: '确定',
                      cancelButtonText: '取消',
                      type: 'warning'
                    }).then(() => {
                      this.$axios.post("/mDesignProcedure/addProcedure.action",JSON.stringify(arr)
                        ,{headers:{"Content-Type":"application/json"}}
                      ).then(function (response) {}).catch();
                      getdata();
                      this.$message({
                        type: 'success',
                        message: '设计成功!'
                      });
                      this.zdwinshow = false;
                    }).catch(() => {
                      this.$message({
                        type: 'info',
                        message: '已取消设计'
                      });
                    });




                  }
                },
                created() {
                  this.getdata();
                  var _this = this; //声明一个变量指向Vue实例this，保证作用域一致
                    _this.currentTime = //修改数据date
                      new Date().getFullYear() +
                      "-" +
                      (new Date().getMonth() + 1) +
                      "-" +
                      new Date().getDate() +
                      " " +
                      new Date().getHours() +
                      ":" +
                      new Date().getMinutes() +
                      ": " +
                      new Date().getSeconds();
                }
              }
          </script>

          <style>
            .xhx {
              height: 25px;
              border: none;
              border-bottom: 1px solid black;
              background-color: transparent;
              outline: none;
            }
            .mbk{
              border: none;
              height: 20px;
            }
          </style>
