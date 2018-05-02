<template>
  <div class="content">
    <section class="process">
      <el-steps :active="1">
        <el-step title="订单初始化" :description=user+date></el-step>
        <el-step title="销售审核" ></el-step>
        <el-step title="技术审核" ></el-step>
        <el-step title="订单排单" ></el-step>
      </el-steps>
    </section>

    <el-button type="primary" size="medium" class="addBtn" @click="addData"><i class="el-icon-edit"></i>添加</el-button>
    <el-button type="primary" size="medium" class="exportBtn" @click="mergeTrue"  ><i class="el-icon-view"></i>打印预览</el-button>
    <el-button type="primary" size="medium" class="exportBtn" @click="handleDownload" ><i class="el-icon-download"></i>导出表格</el-button>
    
    <div v-show="showTable">
      <el-table class="elTable" :data="tableHeaderdata" border style="width:921px;" >
        <el-table-column
          label="发文部门"
          width="140"
          prop="indentIdCN"
          >

        </el-table-column>
        
        <el-table-column label="销售部" width="300">
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

      <el-table class="elTable mainTable"
        :data="tableData"
        border
        stripe
        v-loading.body="listLoading" 
        :span-method='objectSpanMethod'
        >
        
              <el-table-column label="物料编码" width="120"  >
                <template slot-scope="scope" >
                    <template v-if="scope.row.edit">
                      <el-input v-model.trim="scope.row.materialsId" placeholder="请输入内容"></el-input>
                    </template>
                    <span v-else>{{scope.row.materialsId}}</span>
                </template>
              </el-table-column>
        
        
                <el-table-column
                  label="产品名称"
                  width="180"
                  prop="ProductName"
                >
                </el-table-column>
                <el-table-column
                  label="配件要求"
                  width="280"
                >
                  <el-table-column
                    label="型号"
                    width="200"
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
                  width="180"
                  prop="packingWay"
                >
                <template slot-scope="scope" >
                    <template v-if="scope.row.edit">
                    <el-input v-model.trim="scope.row.packingWay" placeholder="请输入内容"></el-input>
                    </template>
                    <span v-else>{{scope.row.packingWay}}</span>
                </template>
                </el-table-column>
                <el-table-column
                  label="配件要求"
                  width="580"
                >
                  <el-table-column
                    label="表号"
                    width="180"
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
                    width="400"
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
            <el-button v-if="scope.row.edit == false" type="danger" @click='deleData(scope.row)' size="small" icon="el-icon-delete">删除</el-button>
          </template>
        </el-table-column>

      </el-table>

      <div class="subFuc">
        <el-button type="primary" size="medium" class="submit"><i class="el-icon-check"></i>提交审核</el-button>
        <el-button  size="medium" class="cancel"   ><i class="el-icon-close"></i>取消</el-button>
      </div>

      <el-dialog title="打印预览" :visible.sync="dialogVisible"  width="1630px" :before-close="handleClose">
        <div id="print">
          <div class="title">
            <h1>温岭甬岭水表有限公司</h1>
            <h1 class="">生产任务通知单</h1>
          </div>
          <el-table class="elTable" :data="tableData"border stripe :span-method="objectSpanMethod" >
                <el-table-column
                  label="发文部门"
                  width="110">
                  <el-table-column
                    label="订单编号"
                    width="110">
                      <el-table-column label="物料编码" width="110"  >
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
                  width="400">
                    <el-table-column
                    :label="this.tableHeaderdata[0].indentId"
                    width="400">
                        <el-table-column
                          label="产品名称"
                          width="260"
                          prop="ProductName"
                        >
                        </el-table-column>
                        <el-table-column
                          label="配件要求"
                          width="240"
                        >
                          <el-table-column
                            label="型号"
                            width="180"
                            prop="type"
                          >
                          </el-table-column>
                          <el-table-column
                            label="脉冲"
                            width="60"
                            prop="pulse"
                          >
                          </el-table-column>
                        </el-table-column>
                    </el-table-column>
                </el-table-column>

                <el-table-column
                  label="受文部门"
                  width="140">
                  <el-table-column
                    label="要求完成日期"
                    width="140">
                      <el-table-column
                        label="单位"
                        width="60"
                        prop="unit"
                      >
                      </el-table-column>
                      <el-table-column
                        label="数量"
                        width="80"
                        prop="number"
                      >
                      </el-table-column>
                  </el-table-column>
                </el-table-column>
                
                <el-table-column
                  label="生产部、品管部"
                  width="600">
                    <el-table-column
                    :label="this.tableHeaderdata[0].date"
                    width="600">
                        <el-table-column
                          label="包装方式"
                          width="200"
                          prop="packingWay"
                        >
                        </el-table-column>
                        <el-table-column
                          label="配件要求"
                          width="480"
                        >
                          <el-table-column
                            label="表号"
                            width="180"
                            prop="tableNumber"
                          >
                          </el-table-column>
                          <el-table-column
                            label="配件"
                            width="300"
                            prop="require"
                          >
                          </el-table-column>
                        </el-table-column>
                    </el-table-column>
                </el-table-column>             
            
          </el-table>
          <div class="othersRequire">
              <span >其他要求:</span>
              <input  class="othersInput" v-model="tableHeaderdata.others" placeholder="在此输入内容"></input>
          </div> 
          <div class="tableDate">
              <span>制单/日期：<input  class="nameInput" v-model="tableHeaderdata.name" placeholder="请输入"></input><span>{{ date }}</span></span> 
              <div class="allow">
                <p class="allowTitle">批准/日期：</p>
                <p class="allowTitle">技术部批准/日期：</p>
                <p class="allowTitle">生产部批准/日期：</p>
              </div>
          </div>
        </div>             
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="setPrint">确定</el-button>
          <el-button @click="dialogVisible = false">取消</el-button>
        </span>  
      </el-dialog>

    </div>

    <div id = "storeCanvas" class="storeCanvas">
    </div>
  </div>
</template>

<script>
import html2canvas from "html2canvas";
import printJs from "print-js";
export default {
  created() {
    this.getMergeNumber();
    this.getList();
    this.fetchData()
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
    getMergeNumber(merginColumn) {
      if (this.mergeShow === true) {
          for ( let i = 0, j = 1;  i < this.tableData.length, j < this.tableData.length; i++, j++  ) {
            if ( this.tableData[i].ProductName == this.tableData[i + 1].ProductName && this.tableData[i].ProductName != " "  ) {
              //去重复
              if (this.merge.indexOf(i) === -1) {
                this.merge.push(i); //merge加入i
                this.merge = this.merge.sort(this.mergeSort);
              }
              //去重复
              if (this.mergeStart.indexOf(i) === -1) {
                this.mergeStart.push(i); //mergeStart加入i
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
          }
      // column为  packingWay
          for ( let i = 0, j = 1;  i < this.tableData.length, j < this.tableData.length; i++, j++  ) {
            if ( this.tableData[i].packingWay == this.tableData[i + 1].packingWay && this.tableData[i].packingWay != " "  ) {
              //去重复
              if (this.mergePack.indexOf(i) === -1) {
                this.mergePack.push(i); //merge加入i
                this.mergePack = this.mergePack.sort(this.mergeSort);
              }
              //去重复
              if (this.mergePackStart.indexOf(i) === -1) {
                this.mergePackStart.push(i); //mergeStart加入i
                this.mergePackStart = this.mergePackStart.sort(this.mergeSort);
              }
            }
          }

          if (this.mergePack.length > 0) {
            // 添加合并数组对象
            this.storePackMerge = this.getMergeObj(this.mergePack);
            for (let i = 0; i < this.storePackMerge.length; i++) {
              let location = this.storePackMerge[i].length - 1;
              this.storePackMerge[i].push(this.storePackMerge[i][location] + 1);
            }
          }

          if (this.mergePackStart.length > 0) {
            // 添加合并数组对象
            this.mergePackIndex = this.getMergeObj(this.mergePackStart);
            for (let i = 0; i < this.mergePackIndex.length; i++) {
              let location = this.mergePackIndex[i].length - 1;
              this.mergePackIndex[i].push(this.mergePackIndex[i][location] + 1);
              this.mergePackIndex[i].shift();
            }
          }
          // column为  require
          for ( let i = 0, j = 1;  i < this.tableData.length, j < this.tableData.length; i++, j++  ) {
            if ( this.tableData[i].require == this.tableData[i + 1].require && this.tableData[i].require != " "  ) {
              //去重复
              if (this.mergeRequire.indexOf(i) === -1) {
                this.mergeRequire.push(i); //merge加入i
                this.mergeRequire = this.mergeRequire.sort(this.mergeSort);
              }
              //去重复
              if (this.mergeRequireStart.indexOf(i) === -1) {
                this.mergeRequireStart.push(i); //mergeStart加入i
                this.mergeRequireStart = this.mergeRequireStart.sort(this.mergeSort);
              }
            }
          }

          if (this.mergeRequire.length > 0) {
            // 添加合并数组对象
            this.storeRequireMerge = this.getMergeObj(this.mergeRequire);
            for (let i = 0; i < this.storeRequireMerge.length; i++) {
              let location = this.storeRequireMerge[i].length - 1;
              this.storeRequireMerge[i].push(this.storeRequireMerge[i][location] + 1);
            }
          }

          if (this.mergeRequireStart.length > 0) {
            // 添加合并数组对象
            this.mergeRequireIndex = this.getMergeObj(this.mergeRequireStart);
            for (let i = 0; i < this.mergeRequireIndex.length; i++) {
              let location = this.mergeRequireIndex[i].length - 1;
              this.mergeRequireIndex[i].push(this.mergeRequireIndex[i][location] + 1);
              this.mergeRequireIndex[i].shift();
            }
          }
      }
    },
    getMergeObj(arr) {
      if (this.mergeShow === true) {
        // 获取合并数组对象
        let result = [],
          i = 0;
        result[i] = [arr[0]];
        arr.reduce(function(prev, cur) {
          cur - prev === 1 ? result[i].push(cur) : (result[++i] = [cur]);
          return cur;
        });
        return result;
      }
    },
    objectSpanMethod({ row, column, rowIndex, columnIndex }) {
      if (this.mergeShow === true) {
        for (let j = 0; j < this.storeMerge.length; j++) {
          if (columnIndex === 1) {
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
        // column为packingWay
        for (let j = 0; j < this.storeMerge.length; j++) {
          if (columnIndex === 6) {
            // 第几列
            if (this.storePackMerge.length > 0) {
              let rowIndexStr = "";
              for (let i = 0; i < this.mergePackIndex.length; i++) {
                if (i < this.mergePackIndex.length - 1) {
                  rowIndexStr =
                    rowIndexStr + "rowIndex===" + this.mergePackIndex[i] + "||";
                } else {
                  rowIndexStr += "rowIndex===" + this.mergePackIndex[i];
                }
              }
              rowIndexStr = rowIndexStr.replace(/,/gi, "||rowIndex===");
              let sumRowspan = this.storePackMerge[j].length; // 合并的行数
              if (rowIndex == this.storePackMerge[j][0]) {
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
        // column为 require
        for (let j = 0; j < this.storeRequireMerge.length; j++) {
          if (columnIndex === 8) {
            // 第几列
            if (this.storeRequireMerge.length > 0) {
              let rowIndexStr = "";
              for (let i = 0; i < this.mergeRequireIndex.length; i++) {
                if (i < this.mergeRequireIndex.length - 1) {
                  rowIndexStr =
                    rowIndexStr + "rowIndex===" + this.mergeRequireIndex[i] + "||";
                } else {
                  rowIndexStr += "rowIndex===" + this.mergeRequireIndex[i];
                }
              }
              rowIndexStr = rowIndexStr.replace(/,/gi, "||rowIndex===");
              let sumRowspan = this.storeRequireMerge[j].length; // 合并的行数
              if (rowIndex == this.storeRequireMerge[j][0]) {
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
    getDate() {
      let date = new Date();
      this.localeDate = date.toLocaleDateString();
      return this.localeDate
    },
    setPrint() {
      let print = document.getElementById("print");
      let storeCanvas = document.getElementById("storeCanvas");

      html2canvas(print).then(function(canvas) {
        storeCanvas.appendChild(canvas);
        let canvasPNG = document.getElementById("storeCanvas").firstChild;
        let img = canvasPNG.toDataURL("image/png");
        document.body.innerHTML ='<button  onclick="window.location.reload()" >返回</button><br/><img  src="' + img +'"/>';
        printJS(img, "image");
      });
    },
    exportTable() {},
    mergeTrue() {
      this.dialogVisible = true;

      this.mergeShow = true;
      this.getMergeNumber();
      this.getMergeObj();
      this.objectSpanMethod();
      Print({
        printable: "print",
        type: "html",
        // 继承原来的所有样式
        targetStyles: ["*"]
      });
      this.$message({
        message: "开始合并单元格",
        type: "success"
      });
    },
    mergeSort(a, b) {
      return a - b;
    },
    addData() {
      this.tableData.push({
        materialsId: " ",
        ProductName: " ",
        type: " ",
        pulse: " ",
        number: null,
        require: " ",
        tableNumber: " ",
        unit: " ",
        packingWay: ""
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
    deleData(row) {
      let rowIndex = this.tableData.indexOf(row);
      if (rowIndex !== -1) {
        this.tableData.splice(rowIndex, 1);
        this.edit = !this.edit;
        this.getMergeNumber();
        this.$message({
          message: "添加成功 " + rowIndex,
          type: "success"
        });
      } else {
        this.$message.error("删除失败,请重新尝试");
      }
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
    },
    handleClose(done) {
      this.$confirm("确认关闭？")
        .then(_ => {
          done();
        })
        .catch(_ => {});
    },
    fetchData() {
        this.list = this.tableData
    },
    handleDownload() {
      this.downloadLoading = true
      import('@/vendor/Export2Excel').then(excel => {
        const tHeader = ['物料编码', '产品名称', '型号', '脉冲', '单位','数量','包装方式','表号','配件']
        const filterVal = ['materialsId', 'ProductName', 'type', 'pulse','unit', 'number','packingWay','tableNumber','require']
        const list = this.list
        const data = this.formatJson(filterVal, list)
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: this.filename,
          autoWidth: this.autoWidth
        })
      })
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => {
        if (j === 'timestamp') {
          return parseTime(v[j])
        } else {
          return v[j]
        }
      }))
    }
  },
  data() {
    return {
      showTable: true,
      number: 1,
      mergeStart: [],
      merge: [],
      date: this.getDate(),
      user: ' 李小芳 ',
      storeMerge: [],
      mergeIndex: [],
      mergePack: [],
      mergePackStart: [],
      storePackMerge: [],
      mergePackIndex: [],
      mergeRequire: [],
      mergeRequireStart: [],
      storeRequireMerge: [],
      mergeRequireIndex: [],
      mergeShow: false,
      edit: false,
      merginColumn: [{column: 1,name: 'ProductName'},{column: 6,name: 'packingWay'},{column: 8,name: 'require'}],
      dialogVisible: false,
      list: null,
      listLoading: true,
      filename: '生产任务通知单 ' + this.getDate(),
      autoWidth: true,
      value:'',
      tableHeaderdata: [
        {
          indentIdCN: "订单编号",
          indentId: "GX65683",
          dateCN: "要求完成日期",
          date: "5.20 ",
          unit: "只",
          others: " ",
          name: " "
        }
      ],
      tableData: [
        {
          id: 1,
          materialsId: "C010281",
          ProductName: "多流干式蓝塑铜壳（凸台）冷水表40°，预装远传-蓝色表盖（一字型表盖）",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "冷水不含接帽，热水含接帽，铁壳放置橡胶垫片，铅封全部换成蓝色，塑料铅封",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "贴条形码，无合格证，无说明书，中性铅封，排序纸盒，打托盘"
        },
        {
          id: 2,
          materialsId: "C010282",
          ProductName: "多流干式蓝塑铜壳（凸台）冷水表40°，预装远传-蓝色表盖（一字型表盖）",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "冷水不含接帽，热水含接帽，铁壳放置橡胶垫片，铅封全部换成蓝色，塑料铅封",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "贴条形码，无合格证，无说明书，中性铅封，排序纸盒，打托盘"
        },
        {
          id: 3,
          materialsId: "C010283",
          ProductName: "多流干式蓝塑铜壳（凸台）冷水表40°，预装远传-蓝色表盖（一字型表盖）",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "冷水不含接帽，热水含接帽，铁壳放置橡胶垫片，铅封全部换成蓝色，塑料铅封",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "贴条形码，无合格证，无说明书，中性铅封，排序纸盒，打托盘"
        },
        {
          id: 4,
          materialsId: "C010284",
          ProductName: "多流干式蓝塑铜壳（凸台）热水表90°，预装远传-红色表盖（一字型表盖）",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "字面：YL8.314.628,流量内控：1.5（Qn，Qt），3（Qmin）",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "贴条形码，无合格证，无说明书，中性铅封，排序纸盒，打托盘"
        },
        {
          id: 5,
          materialsId: "C010285",
          ProductName: "多流干式蓝塑铜壳（凸台）热水表90°，预装远传-红色表盖（一字型表盖）",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "字面：YL8.314.628,流量内控：1.5（Qn，Qt），3（Qmin）",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "贴条形码，无合格证，无说明书，中性铅封，排序纸盒，打托盘"
        },
        {
          id: 6,
          materialsId: "C010286",
          ProductName: "多流干式蓝塑铜壳（凸台）热水表90°，预装远传-红色表盖（一字型表盖）",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "可拆式字面，YL8.314.599，YL8.314.600，铅封换成蓝色铅封",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "贴条形码，无合格证，无说明书，中性铅封，排序纸盒，打托盘"
        },
        {
          id: 7,
          materialsId: "C010287",
          ProductName: "多流干式蓝塑铁壳法兰式（凸台），冷水表-预装远传-蓝色表盖（一字型表盖）",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "可拆式字面，YL8.314.599，YL8.314.600，铅封换成蓝色铅封",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "复合纸箱，套泡沫盖，套气球袋"
        },
        {
          id: 8,
          materialsId: "C010288",
          ProductName: "DN50-DN200自制/外购可拆式，蓝塑灰铁壳体，冷水50°，流量内控1.5（Qn，Qt）3（Qmin），国标PN10，预装远传，自制360°透明罩/钛美合金钢体盖，外购不同透明罩/普通钢表盖",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "可拆式字面，YL8.314.599，YL8.314.600，铅封换成蓝色铅封",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "复合纸箱，配橡胶垫片，套泡沫盖，套气泡袋，中性铅封，指定铭牌"
        },
        {
          id: 11,
          materialsId: "C0102811",
          ProductName: "整机外购垂直螺翼式打表-冷水50°，蓝塑球墨壳-PN10-巴西法兰标准钛镁合金360°钢表盖-技数器360度预装远传（每只表装2处磁铁）",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "排污表，字面：YL8.314.824，该系列产品表号规则此单起有所更改，请注意，铅封换成蓝色",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "DN50-150复合纸箱，DN200-300胶合板木箱，配橡胶垫片"
        },
        {
          id: 12,
          materialsId: "C0102812",
          ProductName: "单流D5远传线",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "排污表，字面：YL8.314.824，该系列产品表号规则此单起有所更改，请注意，铅封换成蓝色",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "DN50-150复合纸箱，DN200-300胶合板木箱，配橡胶垫片"
        },
        {
          id: 13,
          materialsId: "C0102813",
          ProductName: "甬美多流远传线",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "排污表，字面：YL8.314.824，该系列产品表号规则此单起有所更改，请注意，铅封换成蓝色",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "DN50-150复合纸箱，DN200-300胶合板木箱，配橡胶垫片"
        },
        {
          id: 14,
          materialsId: "C0102814",
          ProductName: "可拆式大表远传线",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "排污表，字面：YL8.314.824，该系列产品表号规则此单起有所更改，请注意，铅封换成蓝色",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "参照上一单打包方式"
        },
        {
          id: 15,
          materialsId: "C0102815",
          ProductName: "可拆式大表远传线",
          type: "LXSG-20D3C/ZD（R80/无接帽/二极磁钢）",
          pulse: "1L",
          number: 7800,
          require: "发货5天内必须提供电子流量记录（目前严重滞后）",
          tableNumber: "壳体编号：E170812201-E170820000",
          unit: "只",
          packingWay: "参照上一单打包方式"
        }
      ]
    };
  }
};
</script>
<style>
.content {
  margin-left: 60px; 
}
.process {
  width: 1412px;
}
.elTable {
  margin: 10px 0 0 60px;
  width: 90%;
}
.mainTable {
  margin-bottom: 20px;
}
.el-table .cell,
.el-table th div {
  text-align: center;
}
.addBtn,.process,.submit {
  margin: 30px 0 0 60px;
}

.othersRequire {
  box-sizing: border-box;
  white-space: normal;
  word-break: break-all;
  line-height: 92.48px;
  padding-left: 10px;
  padding-right: 10px;
  margin-left: 60px;
  width: 1432px;
  height: 92.48px;
  border: 1px solid  #E8EBF4;
  border-top: 0;
}
.othersInput {
  width: 1300px;
  border: 0;
  color: #6a6f77;
}
.tableDate{
  margin: 10px 0 0 70px; 
}
.subFuc {
  margin-bottom: 30px; 
}
.nameInput {
  width: 80px;
  border: 0;
  color: #6a6f77;
}
.allow {
  position: relative;
  left: 960px;
  bottom: 40px;
}
.title>h1 {
  text-align: center;
}
.title {
  margin-bottom: 50px; 
}
#storeCanvas {
  margin: 10px 0 0 60px;
}
</style> 
