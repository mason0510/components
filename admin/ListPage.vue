<template>
    <el-card class="list-page">
        <div slot="header" class="title">
            <h2>{{title || '会员管理'}}</h2>
            <!-- 展示的表单 -->
            <p>
                <el-input
                    v-if="isSearch"
                    :placeholder="placeholder"
                    @keyup.enter.native="searchHandle"
                    @blur="cancelSearch"
                    v-model="searchVal"
                >
                    <el-button @click="searchHandle" slot="append" icon="el-icon-search">搜索</el-button>
                </el-input>
            </p>
            <div class="create" v-if="isCreate">
                <el-button type="primary" size="small" @click="handleCreate">
                    <i class="el-icon-plus"></i> 新建
                </el-button>
            </div>
        </div>
        <el-table :data="tableData" stripe style="width: 100%">
            <template v-for="tableItem in tableTemplate">
                <!-- :prop="tableItem.prop" -->
                <el-table-column :key="tableItem.prop" :label="tableItem.label">
                    <template slot-scope="scope">
                        <span v-if="tableItem.prop === 'bespeakStatus'">
                            <el-tag v-if="scope.row.bespeakStatus === 1">预约中</el-tag>
                            <el-tag v-if="scope.row.bespeakStatus === 2" type="success">预约成功</el-tag>
                            <el-tag v-if="scope.row.bespeakStatus === 3" type="danger">预约失败</el-tag>
                        </span>
                        <span v-else>{{scope.row[tableItem.prop] | formatDate}}</span>
                    </template>
                </el-table-column>
            </template>
            <el-table-column fixed="right" label="操作" width="220">
                <template slot-scope="scope">
                    <el-button
                        v-if="isDetail"
                        @click="handleDetail(scope.row)"
                        type="text"
                        style="color:#84be54;"
                        size="small"
                    >
                        <i class="el-icon-thumb"></i>查看详细
                    </el-button>
                    <!-- 另外的条件，当是预约的情况下，当状态不是预约中的时候直接不渲染 -->
                    <el-button
                        v-if="isEdit || scope.row.bespeakStatus === 1"
                        type="text"
                        size="small"
                        @click="handleEdit(scope.row)"
                    >
                        <i class="el-icon-edit"></i>
                        {{editText}}
                    </el-button>
                    <el-button
                        @click="handleDelete(scope.row)"
                        style="color:red;"
                        type="text"
                        size="small"
                        v-if="isDelete"
                    >
                        <i class="el-icon-delete"></i>删除
                    </el-button>
                </template>
            </el-table-column>
        </el-table>
        <!-- 分页器 -->
        <p class="pagination">
            <el-pagination
                :hide-on-single-page="true"
                @current-change="handleCurrentChange"
                :current-page.sync="page"
                :page-size="pageSize"
                layout="prev, pager, next, jumper"
                :total="total"
            ></el-pagination>
        </p>
    </el-card>
</template>
<script lang="ts">
// import { Component, Prop, Vue, Provide, Emit } from "vue-property-decorator";
// /**
//  * 用来展示数据+数据搜索的一个组件，通用
//  */
// @Component
// export default class ListPage extends Vue {
//     // 1.展示的标题
//     @Prop(String) public title!: string;
//     // 是否删除
//     @Prop({
//         type: Boolean,
//         default: false
//     })
//     public isDelete!: boolean;
//     @Prop({
//         type: Boolean,
//         default: false
//     })
//     public isDetail!: boolean;
//     @Prop({
//         type: Boolean,
//         default: true
//     })
//     public isEdit!: boolean;
//     @Prop({
//         type: Boolean,
//         default: true
//     })
//     public isSearch!: boolean;
//     @Prop({
//         type: Boolean,
//         default: false
//     })
//     public isCreate!: boolean;
//     @Prop({
//         type: String,
//         default: "编辑"
//     })
//     public editText!: string;
//     // 分页数据
//     @Prop({
//         type: Number,
//         default: 1
//     })
//     public defaultPage!: number;
//     @Provide() public page: number = 1;
//     @Prop({
//         type: Number,
//         default: 10
//     })
//     public pageSize!: number;
//     @Prop({
//         type: Number,
//         default: 10
//     })
//     public total!: number;
//     @Prop({
//         type: String,
//         default: "请输入内容搜索"
//     })
//     public placeholder!: string;
//     // 2.展示的数据列表
//     @Prop({
//         type: Array
//     })
//     public tableData!: any[];
//     @Prop({
//         type: Array
//     })
//     public tableTemplate!: any[];
//     @Provide() searchVal: string = "";
//
//     public created() {
//         this.page = this.defaultPage;
//     }
//     // 搜索回调函数
//     public searchHandle() {
//         this.searchVal && this.$emit("searchHandle", this.searchVal);
//     }
//     // 取消查询
//     public cancelSearch(){
//         if(this.searchVal === '') {
//             this.$emit("cancelSearch");
//         }
//     }
//     // 4.各种触发的回调函数
//     public handleDetail(row: any) {
//         this.$emit("handleDetail", row);
//     }
//     public handleEdit(row: any) {
//         this.$emit("handleEdit", row);
//     }
//     public handleDelete(row: any) {
//         this.$confirm("确定要删除当前这条吗？", "删除当前数据", {
//             distinguishCancelAndClose: true,
//             confirmButtonText: "删除",
//             cancelButtonText: "取消",
//             confirmButtonClass: 'error',
//             type: "error"
//         })
//             .then(() => {
//                 this.$emit("handleDelete", row);
//             })
//             .catch(() => this.$message.info('已取消'));
//     }
//     // 3.分页数据情况
//     // 5.分页组件等
//     public handleCurrentChange(page: number) {
//         // console.log(`当前页: ${page}`);
//         this.$emit("handleCurrentChange", page);
//     }
//     @Emit("handleCreate")
//     public handleCreate() {}
// }
</script>
<style>
.list-page .title {
    display: flex !important;
    height: 60px;
}
.list-page .title > h2 {
    flex: 3;
    margin-left: 40px;
}
.list-page .title > p {
    flex: 2;
}
.el-button.error.el-button--primary {
    background-color: red !important;
    border-color: red;
}
.list-page .title .create {
    flex: 1;
    display: flex;
    margin: 20px 0 6px 0;
    justify-content: center;
}
.list-page .pagination {
    text-align: right;
}
</style>