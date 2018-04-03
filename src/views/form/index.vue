<template>
<div>
  <el-select class="typeSelect" v-model="selectValue" placeholder="请选择">
    <el-option v-for="item in options" :key="item.selectValue" :label="item.label" :value="item.selectValue"></el-option>
  </el-select>
  <el-input placeholder="请输入内容" clearable class="searchInput"> </el-input>
  <el-button type="primary" size="medium" class="searchBtn" ><i class="el-icon-search"></i>搜索</el-button>
  <el-button type="primary" size="medium" class="addBtn" @click="addData"><i class="el-icon-edit"></i>添加</el-button>
  <el-button type="primary" size="medium" class="addBtn" @click="downloadTable" ><i class="el-icon-download"></i>导出表格</el-button>
  
  <el-table class="elTable"
    :data="tableData"
    border
    stripe
    v-loading.body="listLoading" 
    :span-method="objectSpanMethod"
    :max-height="windowHeight"  
    >
    <el-table-column
      label="发文部门"
      width="140">
      <el-table-column
        label="订单编号"
        width="140">
          <el-table-column label="物料编码" width="140"  >
            <template slot-scope="scope" >
                <template v-if="scope.row.edit">
                  <el-input v-model="scope.row.materialsId" placeholder="请输入内容"></el-input>
                </template>
                <span v-else>{{scope.row.materialsId}}</span>
            </template>
          </el-table-column>
      </el-table-column>
    </el-table-column>
    
    <el-table-column
      prop="indentId"
      label="销售部"
      width="600">
        <el-table-column
        :label="this.tableHeaderdata.indentId"
        width="600">
            <el-table-column
              label="产品名称"
              width="200"
              prop="ProductName"
            >
            </el-table-column>
            <el-table-column
              label="配件要求"
              width="891"
            >
              <el-table-column
                label="型号"
                width="360"
                prop="type"
              >
              </el-table-column>
              <el-table-column
                label="脉冲"
                width="40"
                prop="pulse"
              >
              </el-table-column>
            </el-table-column>
        </el-table-column>
    </el-table-column>

    <el-table-column
      label="受文部门"
      width="120">
      <el-table-column
        label="要求完成日期"
        width="100">
          <el-table-column
            label="单位"
            width="60"
            prop="unit"
          >
          </el-table-column>
          <el-table-column
            label="数量"
            width="60"
            prop="number"
          >
          </el-table-column>
      </el-table-column>
    </el-table-column>
    
    <el-table-column
      prop="indentId"
      label="生产部、品管部"
      width="787">
        <el-table-column
        :label="this.tableHeaderdata.date"
        width="600">
            <el-table-column
              label="包装方式"
              width="200"
              prop="ProductName"
            >
            </el-table-column>
            <el-table-column
              label="配件要求"
              width="587"
            >
              <el-table-column
                label="表号"
                width="360"
                prop="tableNumber"
              >
              </el-table-column>
              <el-table-column
                label="配件"
                width="718"
                prop="require"
              >
              </el-table-column>
            </el-table-column>
        </el-table-column>
    </el-table-column>   
      
    <el-table-column align="center" label="操作" width="240" >
      <template slot-scope="scope">
          <el-button v-if="scope.row.edit == true" type="success" @click="confirmEdit(scope.row)" size="small" icon="el-icon-circle-check-outline">完成</el-button>
            <el-button v-if="scope.row.edit == false" type="primary" @click='scope.row.edit=!scope.row.edit' size="small" icon="el-icon-edit">编辑</el-button>
            <el-button v-if="scope.row.edit == false" type="danger" @click='deleData(scope.row.id-1)' size="small" icon="el-icon-edit">删除</el-button>

      </template>
    </el-table-column>

  </el-table>

</div>

</template>

<script>
export default {
  created() {
    this.getMergeNumber()
    this.getList()

  },    
  methods: {
    getMergeNumber() {
      for(let i=0;i<this.tableData.length;i++) {
        for(let j=i+1;j<this.tableData.length;j++) {
          if(this.tableData[i].require == this.tableData[j].require) {
            this.merge.push({mergeStart: i,mergEnd: this.merge.length})
          }
        }
      }
    },
    objectSpanMethod({ row, column, rowIndex, columnIndex }) {
      if (columnIndex === 8) {
        //第几列
        if (rowIndex == this.merge[0].mergeStart) {
          //第几行
          return {
            rowspan: this.merge.length, //合并行数
            colspan: 1 //合并的列数
          };
        } else if (rowIndex == 1 || rowIndex == 2 ){
            return {
              rowspan: 0,
              colspan: 0
            };

        }
      }
    },
    handleClose(done) {
      this.$confirm("确认关闭？")
        .then(_ => {
          done();
        })
        .catch(_ => {});
    },
    addData() {
      this.tableData.push({
          id: null,
          materialsId: " ",
          ProductName: " ",
          type: " ",
          pulse: " ",
          number: null,
          require: " ",
          tableNumber: " ",
          unit: " "
      })
        this.listLoading = true
        const items = this.tableData
        this.tableData = items.map(v => {
          this.$set(v, 'edit', false) // https://vuejs.org/v2/guide/reactivity.html

          v.originalTitle = v.edit //  will be used when user click the cancel botton

          return v
        })
        this.listLoading = false
    },
    deleData(id) {
      this.$message({
        message: '操作成功',
        type: 'success'
      })
      this.tableData.splice(id,1)
      this.edit=!this.edit
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.dialogVisible = false;
          this.tableData.push({
            id: this.tableData.length+1,
            materialsId: this.ruleForm2.pass1,
            ProductName: this.ruleForm2.pass2,
            type: " ",
            pulse: ' ',
            number: this.ruleForm2.pass3,
            require: ' ',
            tableNumber: " ",
            unit: ' '
          })
          this.$message({
            message: "数据添加成功",
            type: "success"
          });
        } else {
          this.$message.error("数据添加失败，请重新添加");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },  
    getList() {
      this.listLoading = true
        const items = this.tableData
        this.tableData = items.map(v => {
          this.$set(v, 'edit', false) // https://vuejs.org/v2/guide/reactivity.html

          v.originalTitle = v.edit //  will be used when user click the cancel botton

          return v
        })
        this.listLoading = false
    },
    confirmEdit(row) {
      row.edit = false
      row.originalTitle = row.edit
      this.$message({
        message: '数据修改成功',
        type: 'success'
      })
    },
    getArray() {
      if (this.number === 1) {
        const dataSort = [
          "id",
          "indentId",
          "materialsId",
          "ProductName",
          "type",
          "number",
          "require",
          "tableNumber"
        ];
        const dataName = [
          "序号",
          "订单编号",
          "物料编码",
          "产品名称",
          "规格型号",
          "订单数量",
          "表号要求",
          "配件及其他要求"
        ];
        let start = 0;
        let final = 8;
        for (let j = 0; j < this.tableData.length; j++) {
          //6次
          for (let i = 0; i < dataSort.length; i++) {
            //8次
            let name = dataSort[i];
            this.sum_in.push(this.tableData[j][name]);
          }
          let group = this.sum_in.slice(start, final);
          start += 8;
          final += 8;
          this.sum_out.push(group);
        }
        this.sum_out.unshift(dataName);
        return this.sum_out;
      } else {
        return this.sum_out;
      }
    },
    downloadTable() {
      const rows = this.getArray();
      let csvContent = "data:text/csv;charset=utf-8,";
      rows.forEach(function(rowArray) {
        let row = rowArray.join(",");
        csvContent += row + "\r\n";
      });
      var encodedUri = encodeURI(csvContent);
      var link = document.createElement("a");
      let mydate = new Date().toLocaleString();
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", "生产计划执行单" + " " + mydate + ".csv");
      document.body.appendChild(link);
      link.click();
      this.number++;
    }
  },
  data() {
    var validatePass1 = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入订单编号"));
      } else if (value.length < 5) {
        callback(new Error("请输入正确订单编号"));
      } else {
        callback();
      }
    };
    var validatePass2 = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入物料编码"));
      } else if (value.length < 4) {
        callback(new Error("请输入正确物料编码"));
      } else {
        callback();
      }
    };
    var validatePass3 = (rule, value, callback) => {
      if (!value) {
        callback(new Error("请输入订单数量"));
      } else if (value < 0 && value > 100000000) {
        callback(new Error("请输入正确订单数量"));
      } else {
        callback();
      }
    };
    return {
      list: null,
      listLoading: true,
      number: 1,
      mergeStart: null,
      mergeEnd: null,
      sum_in: [],
      sum_out: [],
      windowHeight: 1300,
      block: [],
      merge: [],
      edit: false,
      tableHeaderdata: {
          indentId: "C-A-HID-6-180203",
          date: "2018.4.25",
          unit: '只'
      },
      tableData: [
        {
          id: 1,
          materialsId: "C0102878",
          ProductName: "光电直读智能基表",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: '1L',
          number: 7800,
          require:
            "冷水不含接帽，热水含接帽，铁壳放置橡胶垫片，铅封全部换成蓝色塑料铅封，字面：YL8.314.628,流量内控：正负1.5（Qn,Qt） 正负3（Qmin）",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: '只'
        },
        {
          id: 2,
          materialsId: "C0124200",
          ProductName: "预装远传水表ControlAgua",
          pulse: '1L',
          type:
            "LXSY-13D/S/F（大小头/R160/10L/脉冲/客户商标黑色表盖/13D6a带阀NPT接管+两角塑料螺帽）",
          number: 15000,
          require:
            "冷水不含接帽，热水含接帽，铁壳放置橡胶垫片，铅封全部换成蓝色塑料铅封，字面：YL8.314.628,流量内控：正负1.5（Qn,Qt） 正负3（Qmin）",
          tableNumber:
            "字面刻表号：18 000001-18 015000，与条形码需要一一对应；塑料铅封号：446842-461841",
          unit: '只'
        },
        {
          id: 3,
          materialsId: "C0101313",
          pulse: '1L',
          ProductName: "SIMAR",
          type: "LXS-25D1（SIMAR蓝色表盖）",
          number: 300,
          unit: '只',
          require: "冷水不含接帽，热水含接帽，铁壳放置橡胶垫片，铅封全部换成蓝色塑料铅封，字面：YL8.314.628,流量内控：正负1.5（Qn,Qt） 正负3（Qmin）",
          tableNumber: "18 006562-18 006861"
        },
        {
          id: 4,
          materialsId: "C0102182",
          ProductName: "SIMAR",
          pulse: '1L',
          unit: '只',
          type: "LXSG-20D1（SIMAR蓝色表盖/5位字轮）",
          number: 200,
          require: "字面图纸号：YL8.314.156",
          tableNumber: "18 008862-18 009061"
        },
        {
          id: 5,
          materialsId: "C0102580",
          ProductName: "单流冷水表DEKACT",
          pulse: '1L',
          unit: '只',
          type: "LXSG-32D3（铜本色/黑色六牙指针）",
          number: 100,
          require: "a",
          tableNumber: "字面刻表号：058400301-058400400"
        },
        {
          id: 6,
          materialsId: "C0102584",
          ProductName: "单流冷水表DEKACT",
          pulse: '1L',
          unit: '只',
          type: "LXSG-32D3（铜本色/黑色六牙指针）",
          number: 100,
          require: "提供电子版流量检测记录；单流干式不打铅封；",
          tableNumber: "字面刻表号：058400501-058400600"
        }
      ],
      options: [
        {
          selectValue: "订单编号",
          label: "订单编号"
        },
        {
          selectValue: "物料编码",
          label: "物料编码"
        }
      ],
      selectValue: "",
      ruleForm2: {
        pass1: "",
        pass2: "",
        pass3: "",
        pass4: ""
      },
      rules2: {
        pass1: [{ validator: validatePass1, trigger: "blur" }],
        pass2: [{ validator: validatePass2, trigger: "blur" }],
        pass3: [{ validator: validatePass3, trigger: "blur" }]
      },
      dialogVisible: false
    };
  }
};
</script>
<style>
.elTable {
  margin: 10px 0 0 60px;
  width: 90%;
}
.el-table .cell,
.el-table th div {
  text-align: center;
}
.addBtn,
.searchBtn {
  margin: 30px 0 0 10px;
}
.searchInput {
  width: 380px;
  margin: 30px 0 0 10px;
}
.typeSelect {
  width: 160px;
  margin: 30px 0 0 60px;
}
</style> 
