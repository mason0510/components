<template>
    <div class="header">
        <div class="title">{{title}}</div>
        <div>
            <el-link class="to-login" v-if="noLogin" @click="logout" type="primary">登录</el-link>
            <div v-else class="user">
                <el-popover
                    placement="top"
                    :title="'欢迎管理员:' + infoUser.username"
                    width="100"
                    trigger="hover"
                >
                    <p class="out-login" @click="logout">退出登录</p>
                    <span slot="reference">
                        <el-avatar icon="el-icon-user-solid"></el-avatar>
                    </span>
                </el-popover>
            </div>
        </div>
    </div>
</template>
<script lang="ts">
// import { Component, Vue, Provide, Watch } from "vue-property-decorator";
// import { Getter, Mutation } from "vuex-class";
// import { AdminDataType } from "../../types/AdminType";
//
// @Component
// export default class Header extends Vue {
//     @Mutation("fliterRoutes") filterRoutes!: () => void;
//     @Getter("getUser") infoUser!: AdminDataType;
//     @Mutation("CLEAR_USER") clearUser!: () => void;
//     public title: string = "预约管理系统";
//     // 验证是否登录
//     get noLogin(): boolean {
//         return !this.infoUser.adminId;
//     }
//     @Watch("infoUser", {
//         immediate: true
//     })
//     public updateRoutes() {
//         this.filterRoutes();
//     }
//     public created() {
//         // this.filterRoutes();
//         // 获取一下判断是否登录了
//         if (this.noLogin) {
//             this.$message.warning("还未登录，请先登录。");
//         }
//     }
//     // 退出登录
//     public logout() {
//         this.clearUser();
//         this.$router.push({
//             name: "login"
//         });
//     }
// }
</script>
<style scoped>
.header {
    display: flex;
    height: 60px;
    line-height: 60px;
    border-bottom: 1px solid #f5f5f5;
}
.header > div {
    flex: 1;
}
.header > .title {
    padding: 0 30px;
    flex: 10;
}
.header .to-login {
    padding: 0 20px;
    cursor: pointer;
    color: #3383ff;
}
.header .user {
    padding: 10px;
}
p.out-login {
    margin-top: 30px;
    cursor: pointer;
    color: red;
    text-align: center;
}
</style>