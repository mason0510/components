<template>
    <el-menu
        :default-active="activePath"
        mode="horizontal"
        @select="handleSelect"
        class="header"
        background-color="#fff"
        text-color="#000"
        active-text-color="#ffd04b"
    >
        <el-menu-item disabled>
            <h1>
                <img :src="inforPageData.pageIcon" alt />
                {{inforPageData.pageTitle}}
            </h1>
        </el-menu-item>
        <el-menu-item index="/home">
            <i class="el-icon-s-home"></i>首页
        </el-menu-item>
        <el-menu-item v-show="!noLogin" index="/area">
            <i class="el-icon-menu"></i>场地列表
        </el-menu-item>
        <el-menu-item v-show="!noLogin" index="/information">
            <i class="el-icon-message"></i>消息中心
        </el-menu-item>
        <template v-if="noLogin">
            <el-menu-item index="/login">
                <i class="el-icon-tickets"></i>登录
            </el-menu-item>
            <el-menu-item index="/register">
                <i class="el-icon-s-order"></i>注册
            </el-menu-item>
            <el-menu-item index="/retrieve">找回密码</el-menu-item>
        </template>
        <template v-else>
            <el-menu-item index="/me">
                <i class="el-icon-user"></i>个人中心
            </el-menu-item>
            <el-menu-item index="/bespeak">
                <i class="el-icon-timer"></i>我的预约
            </el-menu-item>
        </template>
        <el-drawer :visible.sync="showMeInformation" :with-header="false">
            <Me :servicePhone="inforPageData.pageServicePhone" @logout="logout" />
        </el-drawer>
    </el-menu>
</template>
<script lang="ts">
// import { Component, Prop, Vue } from "vue-property-decorator";
// import { Mutation } from "vuex-class";
// import Me from "@/components/Me.vue";
// import { UserType } from "../types/UserType";
// import { PageType } from "../types/PageType";
// import { types } from "../store";
// @Component({
//     components: {
//         Me
//     }
// })
// export default class Header extends Vue {
//     @Mutation(types.CLEAR_INFO_USER) clearInfoUser!: () => void;
//     get user() {
//         return this.$store.getters.getInfoUser;
//     }
//     // 当前选中的path
//     public activePath: string = "";
//     get noLogin() {
//         return this.user.userId ? false : true; // 通过是否有数据判断是否登录了
//     }
//     // 请求获取的页面信息
//     @Prop(Object)
//     public inforPageData!: PageType;
//     // 展示个人信息
//     public showMeInformation: boolean = false;
//     public logout() {
//         this.clearInfoUser();
//         this.showMeInformation = false;
//         this.$router.push({
//             name: "home"
//         });
//     }
//     public created() {
//         this.activePath = this.$router.currentRoute.path;
//     }
//     public updated() {
//         this.activePath = this.$router.currentRoute.path;
//     }
//     public handleSelect(key: string, keyPath: string[]) {
//         // Determine whether or not the login path ?
//         const isLoginRoute = key.includes("/login");
//         // Determine whether or not the retrieve path?
//         const isRetrieve = key.includes("/retrieve");
//         // determine whether or not the register path?
//         const isRegister = key.includes("/register");
//         if (key.includes("/me")) {
//             this.showMeInformation = true;
//             return;
//         }
//         if (isLoginRoute) {
//             // pop login dialog
//             this.$emit("clickLogin");
//             return;
//         }
//         if (isRegister) {
//             // pop register dialog
//             this.$emit("clickRegister");
//             return;
//         }
//         if (isRetrieve) {
//             // pop retrieve password dialog
//             this.$emit("clickRetrieve");
//             return;
//         }
//         this.$router.push(key);
//     }
// }
</script>

<style>
.header li.el-menu-item:nth-of-type(5),
.header li.el-menu-item:nth-of-type(6),
.header li.el-menu-item:nth-of-type(7) {
    float: right;
}
.header .el-menu-item h1 {
    margin: 0;
}
.header .el-menu-item > h1 > img {
    width: 50px;
    height: 50px;
    border-radius: 50%;
}
.header .el-menu-item.is-disabled {
    opacity: 1 !important;
}
.header .el-menu-item.is-active {
    color: #4488ff !important;
    border-color: #4488ff !important;
}
.header .el-menu-item h3 img {
    width: 60px;
    height: 60px;
    border-radius: 50%;
}
</style>