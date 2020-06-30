<template>
    <el-card class="me">
        <div slot="header">
            <el-rate :value="4.6" disabled show-score text-color="#ff9900" />
            <h3>个人中心</h3>
            <el-button type="text" @click="openSetting">
                <i class="el-icon-setting"></i> 修改信息
            </el-button>
            <p class="service-phone">客服微信:{{servicePhone}}</p>
        </div>
        <div>
            <el-divider content-position="left">会员编号</el-divider>
            <span>{{user.userId}}</span>
        </div>
        <div>
            <el-divider content-position="left">会员名</el-divider>
            <span>{{user.username}}</span>
        </div>
        <div>
            <el-divider content-position="left">邮箱</el-divider>
            <span>{{user.email}}</span>
        </div>
        <div>
            <el-divider content-position="left">手机号</el-divider>
            <span>{{user.phone}}</span>
        </div>
        <p class="logout">
            <el-button type="danger" @click="logout">退出登录</el-button>
        </p>

        <el-dialog
            title="修改个人信息"
            :visible.sync="isUpdateUser"
            width="50%"
            style="z-index:1000"
            append-to-body
            :before-close="handleClose"
        >
            <Register :isUserSetting="true" @updateInfoMessage="updateInfoMessage" />
        </el-dialog>
    </el-card>
</template>
<script lang="ts">
// import { Component, Vue, Prop } from "vue-property-decorator";
// import { Getter } from "vuex-class";
// import Register from "./Register.vue";
// import { UserType } from "../types/UserType";
//
// @Component({
//     components: {
//         Register
//     }
// })
// export default class Me extends Vue {
//     // 通过store获取，这里先模拟
//     @Getter("getInfoUser")
//     public user!: UserType;
//     @Prop({
//         type: String,
//         default: ""
//     })
//     public servicePhone!: string;
//     public isUpdateUser: boolean = false;
//     public logout() {
//         this.$confirm("退出登录", "是否退出登录?", {
//             confirmButtonText: "退出",
//             cancelButtonText: "取消"
//         }).then(() => {
//             this.$emit("logout");
//         });
//     }
//     public openSetting() {
//         this.isUpdateUser = true;
//     }
//     public handleClose() {
//         this.isUpdateUser = false;
//     }
//     public updateInfoMessage(user: UserType) {
//         // 发送请求修改个人信息
//         this.$axios
//             .put(
//                 `/user/update`,
//                 // this.updateUser(this.user, user)
//                 user
//             )
//             .then((res: any) => {
//                 if (res.flag) {
//                     this.$message.success(res.message);
//                     this.handleClose();
//                 } else throw new Error(res.message);
//             })
//             .catch(this.catchErrMsg);
//     }
    // 这个方法只加到user对象中需要修改的数据
    // public updateUser(lastUser: UserType, nextUser: UserType) {
    //     let updateUser: UserType;
    //     updateUser.userId = lastUser.userId; // 默认加入userId
    //     Object.keys(lastUser).forEach(key => {
    //         if (nextUser[key] !== lastUser[key]) {
    //             updateUser[key] = nextUser[key];
    //         }
    //     });
    //     return updateUser;
//     }
// }
</script>
<style scoped>
.me h3 {
    margin-bottom: 6px;
}
.me div span {
    padding-left: 80px;
}
.me div span:nth-of-type(1) {
    padding-left: 40px;
    font-size: 14px;
}
.me div .service-phone {
    text-align: right;
    font-size: 13px;
}
.me div .el-rate {
    float: right;
}
.me p.logout {
    padding-top: 30px;
    text-align: center;
}
.me p.logout .el-button {
    width: 250px;
}
</style>