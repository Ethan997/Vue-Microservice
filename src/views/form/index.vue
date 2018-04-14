<template>
  <div>
    <el-button type="primary" size="medium" class="addBtn" @click="addData"><i class="el-icon-edit"></i>添加</el-button>
    <el-button type="primary" size="medium" class="exportBtn" @click="mergeTrue"  ><i class="el-icon-download"></i>打印预览</el-button>
    <el-button type="primary" size="medium" class="exportBtn"  ><i class="el-icon-download"></i>导出表格</el-button>

    <el-table class="elTable"
      :data="tableHeaderdata"
      border
      v-loading.body="listLoading" 
      :max-height="windowHeight"  
      style="width:921px;"
      >
      <el-table-column
        label="发文部门"
        width="140"
        prop="indentIdCN"
        >

      </el-table-column>
      
      <el-table-column
        prop="indentId"
        label="销售部"
        width="300">
          <template slot-scope="scope" >
              <template v-if="scope.row.edit">
                  <el-input v-model.trim="scope.row.indentId" placeholder="请输入内容"></el-input>
              </template>
              <span v-else>{{scope.row.indentId}}</span>
          </template>
      </el-table-column>

      <el-table-column
        label="受文部门"
        width="120"
        prop="dateCN"  
          >

      </el-table-column>
      
      <el-table-column
        label="生产部、品管部"
        width="200">
          <template slot-scope="scope" >
              <template v-if="scope.row.edit">
                  <el-input v-model.trim="scope.row.date" placeholder="请输入内容"></el-input>
              </template>
              <span v-else>{{scope.row.date}}</span>
          </template>
      </el-table-column>   
        
      <el-table-column align="center" label="操作" width="160" >
        <template slot-scope="scope">
          <el-button v-if="scope.row.edit == true" type="success" @click="confirmEdit(scope.row)" size="small" icon="el-icon-circle-check-outline">完成</el-button>
          <el-button  v-else type="primary" @click='setEdit(scope.row)' size="small" icon="el-icon-edit">编辑</el-button>
        </template>
      </el-table-column>

    </el-table>    

    <el-table class="elTable"
      :data="tableData"
      border
      stripe
      v-loading.body="listLoading" 
      :max-height="windowHeight"  
      :span-method='objectSpanMethod'
      >
      
            <el-table-column label="物料编码" width="140"  >
              <template slot-scope="scope" >
                  <template v-if="scope.row.edit">
                    <el-input v-model.trim="scope.row.materialsId" placeholder="请输入内容"></el-input>
                  </template>
                  <span v-else>{{scope.row.materialsId}}</span>
              </template>
            </el-table-column>
      
      
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
                  width="80"
                  prop="pulse"
                >
                  <template slot-scope="scope" >
                      <template v-if="scope.row.edit">
                      <el-input v-model.trim="scope.row.pulse" placeholder="请输入内容"></el-input>
                      </template>
                      <span v-else>{{scope.row.pulse}}</span>
                  </template>
                </el-table-column>
              </el-table-column>



            <el-table-column
              label="单位"
              width="80"
            >
              <template slot-scope="scope" >
                  <template v-if="scope.row.edit">
                  <el-input v-model.trim="scope.row.unit" placeholder="请输入内容"></el-input>
                  </template>
                  <span v-else>{{scope.row.unit}}</span>
              </template>
            </el-table-column>
            <el-table-column
              label="数量"
              width="100"
            >
              <template slot-scope="scope" >
                  <template v-if="scope.row.edit">
                  <el-input v-model.trim="scope.row.number" placeholder="请输入内容"></el-input>
                  </template>
                  <span v-else>{{scope.row.number}}</span>
              </template>
            </el-table-column>

      
              <el-table-column
                label="包装方式"
                width="200"
                prop="ProductName"
              >
              <template slot-scope="scope" >
                  <template v-if="scope.row.edit">
                  <el-input v-model.trim="scope.row.ProductName" placeholder="请输入内容"></el-input>
                  </template>
                  <span v-else>{{scope.row.ProductName}}</span>
              </template>
              </el-table-column>
              <el-table-column
                label="配件要求"
                width="587"
              >
                <el-table-column
                  label="表号"
                  width="360"
                >
                  <template slot-scope="scope" >
                      <template v-if="scope.row.edit">
                      <el-input v-model.trim="scope.row.tableNumber" placeholder="请输入内容"></el-input>
                      </template>
                      <span v-else>{{scope.row.tableNumber}}</span>
                  </template>
                </el-table-column>
                <el-table-column
                  label="配件"
                  width="718"
                  prop="require"
                >
                  <template slot-scope="scope" >
                      <template v-if="scope.row.edit">
                      <el-input v-model.trim="scope.row.require" placeholder="请输入内容"></el-input>
                      </template>
                      <span v-else>{{scope.row.require}}</span>
                  </template>
                </el-table-column>
              </el-table-column>
    
        
      <el-table-column align="center" label="操作" width="240" fixed="right" >
        <template slot-scope="scope">
          <el-button v-if="scope.row.edit == true" type="success" @click="confirmEdit(scope.row);" size="small" icon="el-icon-circle-check-outline">完成</el-button>
          <el-button v-if="scope.row.edit == false" type="primary" @click='scope.row.edit=!scope.row.edit' size="small" icon="el-icon-edit">编辑</el-button>
          <el-button v-if="scope.row.edit == false" type="danger" @click='deleData(scope.row.id-1)' size="small" icon="el-icon-delete">删除</el-button>
        </template>
      </el-table-column>

    </el-table>
  </div>
</template>

<script>
export default {
  created() {
    this.getMergeNumber();
    this.getList();
  },
  methods: {
    /**
  *
  方法说明：得到具有相同内容的可合并rowIndex数组
  *
  @method getMergeNumber
  *
  @param  
  merge: 所有的可合并的rowIndex数组，如【0,1,2,4,5,6,7,8,9】
  storeMerge：具有相同内容的可合并rowIndex数组，如[[0,1,2],[4,5,6],[7,8,9]]
  *
  @return Array
  */
    getMergeNumber(column) {
      //查找合并行数和列数
      for (
        let i = 0, j = 1;
        i < this.tableData.length, j < this.tableData.length;
        i++, j++
      ) {
        if (
          this.tableData[i].require == this.tableData[i + 1].require &&
          this.tableData[i].require != " "
        ) {
          if (this.merge.indexOf(i) === -1) {
            //去重复
            this.merge.push(i); //merge加入i
            this.merge = this.merge.sort(this.mergeSort);
          }
        }
      }
      for (
        let i = 0, j = 1;
        i < this.tableData.length, j < this.tableData.length;
        i++, j++
      ) {
        if (
          this.tableData[i].require == this.tableData[i + 1].require &&
          this.tableData[i].require != " "
        ) {
          if (this.mergeStart.indexOf(i) === -1) {
            //去重复
            this.mergeStart.push(i); //merge加入i
            this.mergeStart = this.mergeStart.sort(this.mergeSort);
          }
        }
      }

      if (this.merge.length > 0) {
        // 添加合并数组对象
        this.storeMerge = this.getMergeObj(this.merge);
        for (let i = 0; i < this.storeMerge.length; i++) {
          let location = this.storeMerge[i].length - 1;
          this.storeMerge[i].push(this.storeMerge[i][location] + 1);
        }
      }

      if (this.mergeStart.length > 0) {
        // 添加合并数组对象
        this.mergeIndex = this.getMergeObj(this.mergeStart);
        for (let i = 0; i < this.mergeIndex.length; i++) {
          let location = this.mergeIndex[i].length - 1;
          this.mergeIndex[i].push(this.mergeIndex[i][location] + 1);
          this.mergeIndex[i].shift();
        }
        // if(this.mergeIndex.length >0 ) {
        //   for(let i=0;i<this.mergeIndex.length;i++) {
        //       this.mergeIndex=this.mergeIndex.concat(this.mergeIndex[i])
        //   }
        // }
      }
    },
    getMergeObj(arr) {
      // 获取合并数组对象
      let result = [],
        i = 0;
      result[i] = [arr[0]];
      arr.reduce(function(prev, cur) {
        cur - prev === 1 ? result[i].push(cur) : (result[++i] = [cur]);
        return cur;
      });
      return result;
    },
    objectSpanMethod({ row, column, rowIndex, columnIndex }) {
      if (this.mergeShow === false) {
        for (let j = 0; j < this.storeMerge.length; j++) {
          if (columnIndex === 8) {
            // 第几列
            if (this.storeMerge.length > 0) {
              let rowIndexStr = "";
              for (let i = 0; i < this.mergeIndex.length; i++) {
                if (i < this.mergeIndex.length - 1) {
                  rowIndexStr =
                    rowIndexStr + "rowIndex===" + this.mergeIndex[i] + "||";
                } else {
                  rowIndexStr += "rowIndex===" + this.mergeIndex[i];
                }
              }
              rowIndexStr = rowIndexStr.replace(/,/gi, "||rowIndex===");
              let sumRowspan = this.storeMerge[j].length; // 合并的行数
              if (rowIndex == this.storeMerge[j][0]) {
                // 第几行开始合并
                return {
                  rowspan: sumRowspan, // 合并的行数
                  colspan: 1 // 合并的列数
                };
              } else if (eval(rowIndexStr)) {
                return {
                  rowspan: 0,
                  colspan: 0
                };
              }
            }
          }
        }
      }
    },
    mergeTrue() {
      this.mergeShow = true;
    },
    mergeSort(a, b) {
      return a - b;
    },
    addData() {
      this.tableData.push({
        id: this.tableData.length + 1,
        materialsId: " ",
        ProductName: " ",
        type: " ",
        pulse: " ",
        number: null,
        require: " ",
        tableNumber: " ",
        unit: " "
      });
      this.listLoading = true;
      const items = this.tableData;
      this.tableData = items.map(v => {
        this.$set(v, "edit", false); // https://vuejs.org/v2/guide/reactivity.html

        v.originalTitle = v.edit; //  will be used when user click the cancel botton

        return v;
      });
      this.listLoading = false;
      this.getMergeNumber();
    },
    deleData(id) {
      this.$message({
        message: "操作成功",
        type: "success"
      });
      this.tableData.splice(id, 1);
      this.edit = !this.edit;
      this.getMergeNumber();
    },
    getList() {
      this.listLoading = true;
      const items = this.tableData;
      const itemsHeader = this.tableHeaderdata;
      this.tableData = items.map(v => {
        this.$set(v, "edit", false);

        v.originalTitle = v.edit;

        return v;
      });
      this.tableHeaderdata = itemsHeader.map(v => {
        this.$set(v, "edit", false);

        v.originalTitle2 = v.edit;

        return v;
      });
      this.listLoading = false;
    },
    confirmEdit(row) {
      row.edit = false;
      row.originalTitle = row.edit;
      this.$message({
        message: "数据修改成功",
        type: "success"
      });
      // 查询产品名称和型号
      this.getMergeNumber();
    },
    setEdit(row) {
      row.edit = !row.edit;
    }
  },
  data() {
    return {
      list: null,
      listLoading: true,
      number: 1,
      mergeStart: [],
      mergeEnd: [],
      sum_in: [],
      sum_out: [],
      windowHeight: 1300,
      block: [],
      merge: [],
      storeMerge: [],
      mergeIndex: [],
      mergeShow: false,
      edit: false,
      tableHeaderdata: [
        {
          indentIdCN: "订单编号",
          indentId: " ",
          dateCN: "要求完成日期",
          date: " ",
          unit: "只"
        }
      ],
      tableData: [
        {
          id: 1,
          materialsId: "C0102878",
          ProductName: "光电直读智能基表",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "a",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只"
        }
        //,
        // {
        //   id: 2,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "a",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 3,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "a",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 4,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "s",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 5,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "s",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 6,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "d",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 7,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "d",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 8,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "d",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 9,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "e",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 10,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "q",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 11,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "q",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 12,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "g",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 13,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "g",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 14,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "g",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // },
        // {
        //   id: 15,
        //   materialsId: "C0102878",
        //   ProductName: "光电直读智能基表",
        //   type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
        //   pulse: "1L",
        //   number: 7800,
        //   require: "w",
        //   tableNumber: "壳体编号：E170812201-E170820000",
        //   unit: "只"
        // }
      ],
      selectValue: "",
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
.addBtn {
  margin: 30px 0 0 60px;
}
</style> 
