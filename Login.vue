<template>
    <div class="login">
        <el-form :model="loginData" :rules="rules" ref="loginData" label-width="100px">
            <el-form-item label="用户名" prop="username">
                <el-input v-model="loginData.username"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
                <el-input type="password" v-model="loginData.password"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('loginData')">登 录</el-button>
                <el-button @click="resetForm('loginData')">重 置</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script lang="ts">
// import { Component, Vue } from "vue-property-decorator";
// import { Mutation } from "vuex-class";
// import FormValidateMixin from "../mixin/formData";
// import { UserType } from "../types/UserType";
// import { types } from "../store";
// import { ResType } from "../types/ResType";
// import ErrorMessageMixin from "../mixin/errMsg";

// @Component({
//     mixins: [FormValidateMixin, ErrorMessageMixin]
// })
// export default class Login extends Vue {
//     @Mutation(types.SET_INFO_USER) setInfoUser!: (val: any) => void;
//     public loginData: UserType = {
//         username: "",
//         password: ""
//     };
//
//     public rules = {
//         username: [
//             { required: true, message: "请输入用户名", trigger: "blur" },
//             {
//                 min: 2,
//                 max: 18,
//                 message: "用户名长度为 2-18 字节之间",
//                 trigger: ["blur", "change"]
//             }
//         ],
//         password: [
//             { required: true, message: "请输入密码", trigger: "blur" },
//             {
//                 min: 6,
//                 max: 16,
//                 message: "密码长度为 6-16 字节之间",
//                 trigger: ["blur", "change"]
//             }
//         ]
//     };
//
//     // 登录
//     public submitForm(ref: string) {
//         (this.$refs[ref] as any).validate((valid: boolean) => {
//             if (!valid) {
//                 return false; // 返回false，触发验证规则
//             }
//             // 发送注册请求等,然后发起mutation
//             this.$axios
//                 .post("/login", this.loginData)
//                 .then((res: ResType & any) => {
//                     if (res.flag) {
//                         res.data && this.setInfoUser(res.data);
//                         this.$message.success(res.message);
//                         this.$emit("loginSuccess");
//                     } else throw new Error(res.message);
//                 })
//                 .catch(this.catchErrMsg);
//         });
//     }
// }
</script>
<style scoped>
.login {
    padding: 10px 60px 10px 20px;
}
</style>