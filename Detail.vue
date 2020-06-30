<template>
    <!-- 展示详情页的组件 -->
    <div class="detail">
        <el-page-header @back="$router.go(-1)">
            <h2 style="margin:0;" slot="content">{{detailTitle}}</h2>
            <div slot="title">返回</div>
        </el-page-header>
        <template v-for="(data,key) in datas">
            <div :key="key" v-if="dataTitle[key]">
                <span>
                    <span class="left">{{dataTitle[key]}}</span>
                    <el-divider direction="vertical"></el-divider>
                    <template v-if="key === 'bespeakStatus'">
                        <p class="right status">
                            <el-tag v-if="data == '1'">预约中</el-tag>
                            <el-tag v-if="data == '2'" type="success">预约成功</el-tag>
                            <el-tag v-if="data == '3'" type="danger">预约失败</el-tag>
                        </p>
                    </template>
                    <p class="right status" v-else-if="key === 'areaImages'">
                        <template v-for="imgURL in data.split(',')">
                            <el-image :key="imgURL" lazy :src="imgURL" :preview-src-list="[imgURL]">
                                <div slot="error" class="image-slot">
                                    <i class="el-icon-picture-outline"></i>
                                </div>
                            </el-image>
                        </template>
                    </p>
                    <p class="right" v-else>{{data | formatDate}}</p>
                </span>
                <el-divider></el-divider>
            </div>
        </template>
        <!-- 展示预约功能 -->
        <p v-if="isBespeak && datas.areaId" class="btn">
            <el-button type="primary" @click="handleBespeak">
                <i class="el-icon-timer"></i>预约
            </el-button>
        </p>
    </div>
</template>
<script lang="ts">
// import { Component, Prop, Vue, Watch } from "vue-property-decorator";
// import ErrorMessageMixin from "../mixin/errMsg";

// @Component({
//     mixins: [ErrorMessageMixin]
// })
// export default class Detail extends Vue {
//     // define one argument detailTitle , parent component pass it on
//     @Prop({
//         type: String,
//         default: "详情页"
//     })
//     detailTitle!: string;
//     // define one argument data , storage object type value
//     public datas: any = {
//         areaId: "场地编号",
//         areaName: "场地名称",
//         areaAddress: "场地地址",
//         areaDescribe:
//             "场地描述,场地描述场地描述场地描述场地描述,场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述场地描述", // 场地介绍
//         areaCost: "100", // 场地费用(元)
//         areaImages:
//             "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1576827856303&di=518cff2befa90ded234dcce79ef9a213&imgtype=0&src=http%3A%2F%2Fa0.att.hudong.com%2F71%2F49%2F01300323911105133807492709208.jpg", // 可以没有
//         areaStartTime: new Date(), // 场地的开放
//         areaEndTime: new Date() // 结束时间
//     };
//     @Prop(String) url!: string;
//     // data title -> dataTitle
//     @Prop(Object) dataTitle!: any;
//
//     // is bespeak ? true or false
//     @Prop({
//         type: Boolean,
//         default: false
//     })
//     isBespeak!: boolean;
//     public handleBespeak() {
//         this.$emit("handleBespeak", { ...this.datas });
//     }
//     @Watch("url", {
//         immediate: true
//     })
//     public watchUrl() {
//         this.getData();
//     }
//     // public created() {
//     //     this.getData();
//     // }
//     private getData() {
//         this.$axios
//             .get(this.url)
//             .then(res => {
//                 // 验证获取数据并渲染出来
//                 this.datas = res.data;
//             })
//             .catch(this.catchErrMsg);
//     }
// }
</script>

<style>
.detail {
    width: 1200px;
    margin: 0 auto;
}
.detail .el-page-header {
    color: #3383ff;
    margin: 20px;
    margin-bottom: 60px;
}
.detail > div > span {
    display: flex;
    align-items: center;
}
.detail > div > span > span.left {
    width: 200px;
    padding-right: 30px;
    text-align: right;
}
.detail > div > span > p.right {
    display: inline-block;
    width: 60%;
    margin: 0;
    padding-left: 30px;
    text-indent: 2em;
    word-break: break-all;
}
.detail > div > span > p.right.status {
    text-indent: 0em;
}
.detail .btn {
    text-align: center;
}
.detail > div > span .el-image {
    display: inline-block;
    width: 100px;
    height: 100px;
    margin: 0 10px;
    border: 1px solid #ccc;
    color: #ccc;
    cursor: pointer;
}
</style>