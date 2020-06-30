<template>
    <!-- 展示数据列表的组件 -->
    <el-card class="list">
        <el-divider class="header">{{title}}</el-divider>
        <div class="search" v-if="isSearch">
            <el-input
                :placeholder="placeholder"
                v-model="searchVal"
                @keyup.enter.native="handleSearch"
                @blur="cancelSearch"
                maxlength="20"
                show-word-limit
            >
                <i class="el-icon-search el-input__icon" slot="suffix"></i>
            </el-input>
        </div>
        <el-table
            :data="tableData"
            style="width: 100%"
            @row-click="handleDetail"
            :show-header="isCustomTableHeader"
        >
            <template v-for="tableHeader in tableHeaderData">
                <el-table-column
                    :key="tableHeader.prop"
                    :prop="tableHeader.prop"
                    :label="tableHeader.label"
                >
                    <template slot-scope="{ row }">
                        <!-- 数据显示,展示 -->
                        <span v-if="row && tableHeader.prop === 'bespeakStatus'">
                            <span>
                                <el-tag v-if="row[tableHeader.prop] == '1'">预约中</el-tag>
                                <el-tag v-if="row[tableHeader.prop] == '2'" type="success">预约成功</el-tag>
                                <el-tag v-if="row[tableHeader.prop] == '3'" type="danger">预约失败</el-tag>
                            </span>
                        </span>
                        <span
                            :class="tableHeader.prop ==='createAt' ? 'times' : ''"
                            v-else
                        >{{row && row[tableHeader.prop] | formatDate}}</span>
                    </template>
                </el-table-column>
            </template>
            <!-- 查看详细、预约功能 -->
            <el-table-column v-if="isBespeak" width="100">
                <template slot-scope="{ row }">
                    <el-button
                        v-if="isBespeak"
                        @click.stop="handleBespeak(row)"
                        type="text"
                        size="small"
                    >
                        <i class="el-icon-timer"></i>预约
                    </el-button>
                </template>
            </el-table-column>
        </el-table>
        <template v-if="isPagination">
            <el-pagination
                class="pagination"
                :hide-on-single-page="true"
                @current-change="handleCurrentChange"
                :current-page.sync="page"
                :page-size="pageSize"
                layout="prev, pager, next, jumper"
                :total="total"
            />
        </template>
    </el-card>
</template>
<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { TableHeaderType } from "../types/TableHeaderType";
@Component
export default class List extends Vue {
    // 1. 定义是否能操作(比如预约) define whether or not be able to operate (suppose bespeak)
    @Prop({
        type: Boolean,
        default: false
    })
    isBespeak!: boolean; // default value false
    // 2. 定义是否能查看详情 define whether or not be able to look detail
    @Prop({
        type: Boolean,
        default: true
    })
    isLookDeatil!: boolean; // default value true
    // 2.1 define table header custom argument default value true
    @Prop({
        type: Boolean,
        default: true
    })
    public isCustomTableHeader!: boolean;
    // 2.2 define table pagination , default value true
    @Prop({
        type: Boolean,
        default: true
    })
    public isPagination!: boolean;
    // 4. 数据数组对象(对应的展示表头行的内容,以及展示的数据) data array object ( corresponding show table header row content , as well as show data)
    @Prop(Array) tableData!: any[]; // show data the array object , passed down by the parent component
    @Prop({ type: Array }) tableHeaderData!: TableHeaderType[];
    // 5. 定义是否可以搜索  define whether or not pretty good search
    @Prop({
        type: Boolean,
        default: false
    })
    isSearch!: boolean; // default value false
    @Prop({
        type: String,
        default: "搜索内容"
    })
    placeholder!: string; // default value '搜索内容'
    public searchVal: string = ""; // search data , default value to ''
    // 8. 分页属性、模糊查询、列表标题 paging properties、fuzzy query 、list title
    @Prop({
        type: Number,
        default: 1
    })
    currentPage!: number; // default current page number
    public page!: number;
    @Prop({
        type: Number,
        default: 10
    })
    pageSize!: number; // default number of entries per page
    @Prop({
        type: Number,
        default: 0
    })
    total!: number; // default total number
    @Prop({
        type: String,
        default: ""
    })
    title!: string; // deafult title value to ''
    public created() {
        this.page = this.currentPage;
    }
    // 3. 定义预约回调函数和查看回调函数 define bespeak callback method and look callback method
    // bespeak callback method
    public handleBespeak(rowData: any) {
        this.$emit("handleBespeak", rowData);
    }
    // look detail callback method
    public handleDetail(rowData: any) {
        this.isLookDeatil && this.$emit("handleDetail", rowData);
    }
    // 6. 搜索回调函数,返回搜索结果 search callback function , return search success
    public handleSearch() {
        if (!this.searchVal) return this.$message.warning("搜索内容不能为空");
        this.$emit("handleSearch", this.searchVal);
    }
    // 7. cancel search trigger callback method
    public cancelSearch() {
        !this.searchVal && this.$emit("cancelSearch");
    }
    // 9. blocked-out page number trigger method and return new page number
    public handleCurrentChange(newPage: number) {
        this.$emit("handleCurrentChange", newPage);
    }
}
</script>

<style scoped>
.list .el-table {
    margin-bottom: 20px;
    cursor: pointer;
}
.list .el-pagination.pagination {
    float: right;
    margin-bottom: 20px;
}
.list .search {
    text-align: right;
    margin-bottom: 25px;
}
.list .search .el-input {
    width: 300px;
}
.list .el-table .times {
    display: inline-block;
    width: 100%;
    text-align: right;
    font-size: 10px !important;
    transform: scale(0.8);
}
</style>