<template>
    <div v-loading="loading">
        <div class="pad_15">
            <el-row>
                <el-col :span="24" class="text_right">
                    <!--<el-button type="primary" size="mini" plain @click="printClick">打印单据</el-button>-->
                    <el-button type="primary" size="mini" plain @click="cancelClick">返回</el-button>
                    <el-button type="primary" size="mini" @click="deliverClick" v-if="isConfirm == '待发货'">确认发货
                    </el-button>
                    <el-button type="primary" size="mini" @click="deliverClick" v-if="isConfirm == '待收货'">确认收货
                    </el-button>
                </el-col>
            </el-row>
        </div>
        <div class="pad_5 backfff">
            <div class="pad_5 nav_header">
                <span class="font_14 marrt_20">销售信息</span>
            </div>
            <div class="nav_header">
                <el-form :inline="true" class="demo-form-inline">
                    <el-form-item label="订单编号">
                        <el-input v-model="billID" disabled size="mini" class="width_220"></el-input>
                    </el-form-item>
                    <el-form-item label="单据状态">
                        <el-input v-model="isConfirm" disabled size="mini"></el-input>
                    </el-form-item>
                    <el-form-item label="客户商">
                        <el-input v-model="clientName" disabled size="mini"></el-input>
                    </el-form-item>
                    <el-form-item label="供应商">
                        <el-input v-model="custName" disabled size="mini"></el-input>
                    </el-form-item>
                    <el-form-item label="发货仓库">
                        <el-input v-model="stockDest" disabled size="mini"></el-input>
                    </el-form-item>
                    <el-form-item label="收货仓库">
                        <el-input v-model="warehouseName" disabled size="mini"></el-input>
                    </el-form-item>
                    <el-form-item label="经手单位">
                        <el-input v-model="entityName" size="mini" disabled></el-input>
                    </el-form-item>
                    <el-form-item label="开单日期">
                        <el-date-picker size="mini" v-model="billDate" align="right" type="date"
                                        disabled></el-date-picker>
                    </el-form-item>
                    <el-form-item label="审核日期">
                        <el-date-picker size="mini" v-model="verifyDate" align="right" type="date"
                                        disabled></el-date-picker>
                    </el-form-item>
                    <el-form-item label="备注">
                        <el-input type="textarea" v-model="remark" disabled placeholder="请输入" size="mini"></el-input>
                    </el-form-item>
                </el-form>
            </div>
        </div>

        <div class="pad_10 backfff martp_15">
            <div class="pad_10 nav_header">
                <span class="font_14 marrt_20">销售明细</span>
                <!--<el-button type="primary" size="mini" icon="el-icon-printer">打印</el-button>-->
            </div>
            <div>
                <el-table
                        :data="tableData"
                        show-summary
                        :summary-method="getSummaries"
                        max-height="400"
                        style="width: 100%"
                >
                    <el-table-column type="index" label="序号"></el-table-column>
                    <el-table-column prop="productName" label="货品名称"></el-table-column>
                    <el-table-column prop="productID" label="货品编号"></el-table-column>
                    <el-table-column prop="disctRate" label="折扣率(%)"></el-table-column>
                    <el-table-column prop="inAmount" label="输入数量"></el-table-column>
                    <el-table-column prop="inUnit" label="选择单位"></el-table-column>
                    <el-table-column prop="maxPrice" label="选择单位单价"></el-table-column>
                    <el-table-column prop="unit" label="最小单位"></el-table-column>
                    <el-table-column prop="price" label="最小单价"></el-table-column>
                    <el-table-column prop="amount" label="最小单位数量合计"></el-table-column>
                    <el-table-column prop="totalPrice" label="合计金额"></el-table-column>
                </el-table>
            </div>
        </div>

        <!--打印显示的东西-->
        <div class="backfff martp_15 none" id="printContent">
            <div class="pad_10">
                <div
                        style="display: flex;justify-content: space-between;font-size: 15px;padding-top: 15px;padding-bottom: 15px;"
                >
                    <div class="con" style="display: flex;width: 50%">
                        <div style="display: inline-block">订单编号：</div>
                        <div style="display: inline-block;padding-left: 20px; ">{{this.billID}}</div>
                    </div>

                    <div style="display: inline-block;width: 50%">
                        <div style="display: inline-block;">开单时间：</div>
                        <div
                                style=" display: inline-block;padding-left: 20px; display: inline-block; padding-right: 20px;"
                        >{{this.billDate}}
                        </div>
                    </div>
                </div>

                <div
                        style="display: flex;justify-content: space-between;font-size: 15px;padding-top: 15px;padding-bottom: 15px;"
                >
                    <div class="con" style="display: inline-block;width: 50%">
                        <div style="display: inline-block;">客户商：</div>
                        <div style="display: inline-block;padding-left: 20px; ">{{this.clientName}}</div>
                    </div>
                    <div class="con" style="display: flex;width: 50%">
                        <div style="display: inline-block;">供应商</div>
                        <div style="display: inline-block;padding-left: 20px;  ">{{this.custName}}</div>
                    </div>
                </div>

                <div
                        style="display: flex;justify-content: space-between;font-size: 15px;padding-top: 15px;padding-bottom: 15px;"
                >
                    <div class="con" style="display: inline-block;width: 50%">
                        <div style="display: inline-block;">发货仓库：</div>
                        <div style="display: inline-block;padding-left: 20px; ">{{this.stockDest}}</div>
                    </div>
                    <div class="con" style="display: flex;width: 50%">
                        <div style="display: inline-block;">收获仓库：</div>
                        <div style="display: inline-block;padding-left: 20px;  ">{{this.warehouseName}}</div>
                    </div>
                </div>
                <div
                        style="display: flex;justify-content: space-between;font-size: 15px;padding-top: 15px;padding-bottom: 15px;"
                >
                    <div style="display: inline-block;width: 50%">
                        <div style="display: inline-block;">制单人：</div>
                        <div
                                style="display: inline-block;padding-left: 20px; padding-right: 20px;"
                        >{{this.accountName}}
                        </div>
                    </div>
                    <div style="display: inline-block;width: 50%">
                        <div style="display: inline-block;">单据状态：</div>
                        <div
                                style="display: inline-block;padding-left: 20px; padding-right: 20px;"
                        >{{this.isConfirm}}
                        </div>
                    </div>
                </div>
                <div
                        style="display: flex;justify-content: space-between;font-size: 15px;padding-top: 15px;padding-bottom: 15px;"
                >
                    <div style="display: inline-block;width: 50%">
                        <div style="display: inline-block;">经手单位：</div>
                        <div
                                style="display: inline-block;padding-left: 20px; padding-right: 20px;"
                        >{{this.entityName}}
                        </div>
                    </div>
                    <div style="display: inline-block;width: 50%">
                        <div style="display: inline-block;">备注：</div>
                        <div
                                style="display: inline-block;padding-left: 20px; padding-right: 20px;"
                        >{{this.remark}}
                        </div>
                    </div>
                </div>
            </div>
            <el-table
                    :data="tableData"
                    show-summary
                    :summary-method="getSummaries"
                    max-height="400"
                    style="width: 100%;"
            >
                <el-table-column prop="productID" label="编号"></el-table-column>
                <el-table-column prop="productName" label="品名"></el-table-column>
                <el-table-column prop="inUnit" label="单位"></el-table-column>
                <el-table-column prop="inAmount" label="数量/本次数量"></el-table-column>
                <el-table-column prop="price" label="销售单价"></el-table-column>
                <el-table-column prop="totalPrice" label="销售金额"></el-table-column>
            </el-table>
        </div>
    </div>
</template>

<script>
    import API from "@/api/salesBusiness";
    import printJS from "print-js";
    export default {
        data() {
            return {
                loading: false,
                selectLoading: false,
                modalLoading: false,
                billID: "",
                stockToType: "",
                isConfirm: "", //单据状态
                clientName: "", //客户商
                custName: "", //供应商
                stockDestID: "", //发货仓库
                stockDest: "",
                warehouseID: "", //收货仓库
                warehouseName: "",
                entityName: "", //经手单位
                billDate: "", //开单日期
                verifyDate: "", // 审核日期
                remark: "", //备注
                tableData: [],
                accountName: "",
            };
        },
        methods: {
            getSummaries(param) {
                const {columns, data} = param;
                const sums = [];
                columns.forEach((column, index) => {
                    if (index === 0) {
                        sums[index] = "合计";
                        return;
                    }
                    if (index == 3 || index == 6 || index == 8 || index == 9) {
                        sums[index] = "";
                        return;
                    }
                    const values = data.map((item) => Number(item[column.property]));
                    if (!values.every((value) => isNaN(value))) {
                        sums[index] = values.reduce((prev, curr) => {
                            const value = Number(curr);
                            if (!isNaN(value)) {
                                return parseFloat((prev + curr).toFixed(2));
                            } else {
                                return prev;
                            }
                        }, 0);
                        if (index === 10) {
                            sums[index] = sums[index].toFixed(2);
                        }
                        // sums[index] += ' 元';
                    } else {
                        // sums[index] = 'N/A';
                    }
                });

                return sums;
            },
            // 单据状态
            formatStatus: function (row) {
                if (row.isConfirm == 3) {
                    return "已完成";
                } else if (row.isConfirm == 5) {
                    return "待发货";
                } else if (row.isConfirm == 6) {
                    return "待收货";
                }
            },
            //单据打印
            printClick() {
                printJS({
                    printable: "printContent",
                    type: "html",
                    scanStyles: false,
                    header: "销售收发货",
                    headerStyle: "text-align:center;font-size:18px",
                    style:
                        "table tr td,th { padding: 15px;text-align:center;} .el-table__footer-wrapper{margin-top:40px}",
                    // targetStyles: ["*"],
                });
            },
            // 取消
            cancelClick: function () {
                var that = this;
                that.$router.go(-1);
            },
            // 数据显示
            handleSearch: function () {
                var that = this;
                that.loading = true;
                var params = {
                    billID: that.billID,
                    stockDestID: that.stockDestID,
                };
                API.receiveDetailList(params)
                    .then((res) => {
                        if (res.stateCode == 100) {
                            res.data.forEach(function (item) {
                                item.barCodeList = item.units;
                                item.quantityAmount = item.stock;
                                item.totalPrice = (
                                    item.inAmount *
                                    item.maxPrice *
                                    item.disctRate
                                ).toFixed(2);
                                item.disctRate = item.disctRate * 100;
                                item.unitAmount = item.unitAmount;
                            });
                            that.tableData = res.data;
                        } else {
                            that.$message.error({
                                message: res.message,
                                duration: 2000,
                            });
                        }
                    })
                    .finally(function () {
                        that.loading = false;
                    });
            },

            // 发货
            deliverClick: function () {
                var that = this;

                this.$confirm("此操作将确认发货, 是否继续?", "提示", {
                    confirmButtonText: "确定",
                    cancelButtonText: "取消",
                    type: "warning",
                })
                    .then(() => {
                        that.loading = true;
                        var params = {
                            billID: that.billID,
                            warehouseID: that.warehouseID,
                            stockDest: that.stockDestID,
                            isConfirm: 5,
                            stockTotype: that.stockToType,
                        };
                        API.getDeliver(params)
                            .then((res) => {
                                if (res.stateCode == 100) {
                                    that.$message({
                                        message: "发货成功",
                                    });
                                    that.$router.go(-1);
                                } else {
                                    that.$message.error({
                                        message: res.message,
                                        duration: 2000,
                                    });
                                }
                            })
                            .finally(function () {
                                that.loading = false;
                            });
                    })
                    .catch(() => {
                        this.$message({
                            type: "info",
                            message: "已取消删除",
                        });
                    });
            },
        },
        mounted() {
            var that = this;
            var detail = that.$route.query.detail;
            that.billID = detail.billID;
            that.stockToType = detail.stockToType;
            that.stockDestID = detail.stockDestID;
            that.stockDest = detail.stockDest;
            that.isConfirm = that.formatStatus(detail) || "";
            that.clientName = detail.clientName;
            that.custName = detail.custName || localStorage.getItem("siteIDName");
            that.warehouseID = detail.warehouseID;
            that.warehouseName = detail.warehouseName;
            that.entityName = detail.entityName;
            that.billDate = detail.billDate;
            that.verifyDate = detail.verifyDate;
            that.remark = detail.remark;
            that.accountName = JSON.parse(localStorage.getItem("access-user")).empName;
            that.handleSearch();
        },
    };
</script>


<style scoped lang="scss">
    .none {
        width: 70%;
        position: absolute;
        top: -10000px;
    }
</style>
