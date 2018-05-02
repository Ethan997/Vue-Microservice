<template>
<div class="addOrder">
    <section class="event">
      <el-row :gutter="12">
        <el-col :span="8">
          <el-card shadow="always">
            <p class="cardTitle">待办订单</p>
            <p class="cardContent">8个订单</p>
          </el-card>
        </el-col>
        <el-col :span="8">
          <el-card shadow="hover">
            <p class="cardTitle">本周订单平均处理时间</p>
            <p class="cardContent">2小时</p>
          </el-card>
        </el-col>
        <el-col :span="8">
          <el-card shadow="never">
            <p class="cardTitle">本周提交订单数</p>
            <p class="cardContent">10个订单</p>
          </el-card>
        </el-col>
      </el-row>
    </section>
    <div class="serchModel">
      <h3>新增订单</h3>
      <section >
        <span>请选择日期：</span>          
        <div class="block">
          <el-date-picker
            v-model="selectDate"
            type="daterange"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            :default-time="['00:00:00', '23:59:59']">
          </el-date-picker>
        </div>
        <el-input placeholder="请输入订单编号" clearable class="searchInput"> </el-input>
        <el-button type="primary" size="medium" class="searchBtn" ><i class="el-icon-search"></i>搜索</el-button>
      </section>
    </div>

    
    <el-table class="elTable"
      :data="tableData"
      border
      stripe
      :span-method="objectSpanMethod"
      :max-height="windowHeight"  
      style="width:1351px"
      >
      <el-table-column
        fixed="left"
        prop="id"
        label="序号"
        width="50">
      </el-table-column>
      <el-table-column
        prop="order_date"
        label="订单编号"
        width="300">
      </el-table-column>
      <el-table-column
      <el-table-column
        prop="order_date"
        label="订单发布时间"
        width="200">
      </el-table-column>
      <el-table-column
        prop="aduit_sale_name"
        label="订单发布人员"
        width="200">
      </el-table-column>
      <el-table-column
        prop="aduit_technology_name"
        label="订单填充人员"
        width="200">
      </el-table-column>
      <el-table-column
        prop="order_arrange_status"
        label="订单状态"
        width="200"
        :filters="[{text:'全部', value: '全部' },{text:'提交审核', value: '提交审核' },{text:'审核未过', value: '审核未过' },{text:'审核通过', value: '审核通过' }]"
        :filter-method="filterTag"
        filter-placement="bottom-end"
        >
        <template slot-scope="scope">
          <el-tag :type="scope.row.order_arrange_state">{{scope.row.order_arrange_status}}</el-tag>
        </template>
      </el-table-column>

      <el-table-column
        fixed="right"
        label="操作"
        width="200">
        <template slot-scope="scope">
          <el-button v-if="scope.row.edit === true" class="dele" size="small" type="danger" plain @click="dialogVisible = true">修改</el-button>
          <el-button v-else class="dele" size="small" type="primary" plain>查看</el-button>
        </template>
      </el-table-column>

    </el-table>

    <div class="block pagination">
      <el-pagination 
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage4"
        :page-sizes="[3]"
        :page-size="10"
        layout="total, sizes, prev, pager, next, jumper"
        :total="3">
      </el-pagination>
    </div>

    <el-dialog
      title="添加新数据"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">
      <el-form :model="ruleForm2" status-icon :rules="rules2" ref="ruleForm2" label-width="120px" class="demo-ruleForm">
        <el-form-item label="订单编号" prop="pass1">
          <el-input type="text" v-model="ruleForm2.pass1" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="物料编码" prop="pass2">
          <el-input type="text" v-model="ruleForm2.pass2" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="产品名称" >
          <el-input type="text" v-model="ruleForm2.pass1" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="规格型号" >
          <el-input type="text" v-model="ruleForm2.pass1" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="物料编码" prop="pass3">
          <el-input type="number" v-model="ruleForm2.pass3" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="配件及其他要求">
          <el-input v-model.number="ruleForm2.pass4" type="textarea"></el-input>
        </el-form-item>
        <el-form-item label="表号要求" >
          <el-input v-model.number="ruleForm2.pass1" type="text" :disabled="true"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submitForm('ruleForm2')">立刻添加</el-button>
        <el-button @click="resetForm('ruleForm2')">重置</el-button>
        <el-button @click="dialogVisible = false">取 消</el-button>
      </span>
    </el-dialog>
</div>

</template>

<script>
  export default {
    methods: {
      formatter(row, column) {
        return row.address;
      },
      filterTag(value, row) {
        return row.order_arrange_status === value;
      },
      objectSpanMethod({ row, column, rowIndex, columnIndex }) {
        if (columnIndex === 7) { //第几列
          if (rowIndex == 4 ) { //第几行
            return {
              rowspan: 2, //合并行数
              colspan: 1 //合并的列数
            };
          } else if(rowIndex == 5) {
            return {
              rowspan: 0,
              colspan: 0
            };
          }
        }
      },
      handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.dialogVisible = false;
            this.$message({
              message: '数据添加成功',
              type: 'success'
            })
          } else {
            this.$message.error('数据添加失败，请重新添加')
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      getArray() {
        if(this.number === 1) {
          const dataSort = ['id','indentId','materialsId','ProductName','type','number','require','tableNumber']
          const dataName = ['序号','订单编号','物料编码','产品名称','规格型号','订单数量','表号要求','配件及其他要求']
          let start = 0;
          let final = 8;
          for(let j=0;j<this.tableData.length;j++) { //6次
            for(let i=0;i<dataSort.length;i++) { //8次
              let name = dataSort[i]
                this.sum_in.push(this.tableData[j][name])
            }
            let group = this.sum_in.slice(start,final)
            start+=8;
            final+=8;
            this.sum_out.push(group)
          }
          this.sum_out.unshift(dataName)       
          return  this.sum_out
        }else{
          return  this.sum_out   
        }
      },
      downloadTable() {
          const rows = this.getArray();
          let csvContent = "data:text/csv;charset=utf-8,";
          rows.forEach(function(rowArray){
            let row = rowArray.join(",");
            csvContent += row + "\r\n";
          }); 
          var encodedUri = encodeURI(csvContent);
          var link = document.createElement("a");
          let mydate = new Date().toLocaleString();
          link.setAttribute("href", encodedUri);
          link.setAttribute("download", "生产计划执行单"+" "+mydate+".csv");
          document.body.appendChild(link); 
          link.click();
          this.number++
      }
    },
    data() {
      var validatePass1 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入订单编号'));
        }else if(value.length < 5) {
          callback(new Error('请输入正确订单编号'));
        } else {
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入物料编码'));
        } else if(value.length < 4) {
          callback(new Error('请输入正确物料编码'));
        } else {
          callback();
        }
      };
      var validatePass3 = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请输入订单数量'));
        } else if(value < 0 && value > 100000000) {
          callback(new Error('请输入正确订单数量'));
        } else {
          callback();
        }
      };
      return {
        selectDate: [],
        number: 1,
        sum_in: [],
        sum_out: [],
        windowHeight: 700,
        block:[],
        totalAge: 10,
        tableData: [
            {
              id: 1,
              order_date: '2018-01-05 15:00',
              aduit_sale_name: '李小芳',
              aduit_technology_name: '周慧芳',
              order_arrange_status: '审核通过',
              order_arrange_state: 'success',
              edit: false
            },
            {
              id: 2,
              order_date: '2018-01-10 16:00',
              aduit_sale_name: '李小芳',
              aduit_technology_name: '周慧芳',
              order_arrange_status: '提交审核',
              order_arrange_state: 'warning',
              edit: true              
            },
            {
              id: 3,
              order_date: '2018-01-15 14:40',
              aduit_sale_name: '李小芳',
              aduit_technology_name: '周慧芳',
              order_arrange_status: '审核未过',
              order_arrange_state: 'danger',
              edit: true              
            },
          ],
          options:[{
            selectValue: "订单编号",
            label: "订单编号"
          },{
            selectValue: "物料编码",
            label: "物料编码"
        }],
        selectValue: '',
        ruleForm2: {
          pass1: '',
          pass2: '',
          pass3: '',
          pass4: ''
        },
        rules2: {
          pass1: [
            { validator: validatePass1, trigger: 'blur' }
          ],
          pass2: [
            { validator: validatePass2, trigger: 'blur' }
          ],
          pass3: [
            { validator: validatePass3, trigger: 'blur' }
          ]
        },
        dialogVisible: false
      }
    }
  }
</script>
<style>
  .addOrder {
    margin-left: 60px; 
  }
  .event {
    width: 1200px;
  }
  .elTable{
    margin: 10px 0 0 0;
    width: 90%;
  }
  .el-table .cell, .el-table th div{
    text-align: center;
  }
  .addBtn, .searchBtn{
    margin: 30px 0 0 10px;
  }
  .searchInput{
    width: 380px;
    margin: 30px 0 0 10px;
  }
  .typeSelect{
    width: 160px;
    margin: 30px 0 0 0;
  }
  .event,.submit {
  margin: 30px 0 0 0;
  }
  .el-card p {
  text-align:center;
  margin: 10px 0 ;
  }
  .cardTitle {
    color: #6a6f77;
  }
  .cardContent {
    font-size: 28px;
  } 
  .el-card__body {
    padding: 10px;
  }
  .serchModel {
    width: 1300px;
    display: flex;
    justify-content: space-between;
  }
  .serchModel h3 {
    line-height: 98px;
    margin: 0;
  }

  .block {
    width: 350px;
    display: inline-block;
  }
  .pagination {
    margin-top: 20px;
    width: 1360px;
    display: flex;
    justify-content: flex-end;
  }

</style> 