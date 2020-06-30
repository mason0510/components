<template>
    <div class="retrieve">
        <el-form :model="retrieveData" :rules="rules" ref="retrieveData" label-width="100px">
            <el-form-item label="邮箱" prop="email">
                <el-input type="email" v-model="retrieveData.email"></el-input>
            </el-form-item>
            <el-form-item label="新密码" prop="newPassword">
                <el-input type="password" v-model="retrieveData.newPassword"></el-input>
            </el-form-item>
            <el-form-item label="验证码" prop="code">
                <el-input v-model="retrieveData.code">
                    <template slot="append">
                        <el-button
                            style="color:#74562c"
                            @click="sendCode('retrieveData')"
                            :disabled="disabledCodeBtn"
                        >{{message}}</el-button>
                    </template>
                </el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('retrieveData')">找 回</el-button>
                <el-button @click="resetForm('retrieveData')">重 置</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script lang="ts">
// import { Component, Vue } from "vue-property-decorator";
// import FormValidateMixin from "../mixin/formData";
// import { UserType } from "../types/UserType";
// import { ResType } from "../types/ResType";
// import ErrorMessageMixin from "../mixin/errMsg";
//
// @Component({
//     mixins: [FormValidateMixin, ErrorMessageMixin]
// })
// export default class Register extends Vue {
//     public retrieveData: UserType = {
//         email: "",
//         newPassword: "",
//         code: ""
//     } as any;
//
//     public rules = {
//         email: [
//             { required: true, message: "请输入邮箱", trigger: "blur" },
//             {
//                 type: "email",
//                 message: "请输入正确的邮箱地址",
//                 trigger: ["blur", "change"]
//             }
//         ],
//         newPassword: [
//             { required: true, message: "新密码不能为空", trigger: "blur" },
//             {
//                 min: 6,
//                 max: 16,
//                 message: "新密码长度为 6-16 字节之间",
//                 trigger: ["blur", "change"]
//             }
//         ],
//         code: [
//             { required: true, message: "请输入验证码", trigger: "blur" },
//             {
//                 min: 5,
//                 max: 5,
//                 message: "密请输入5位验证码",
//                 trigger: ["blur", "change"]
//             }
//         ]
//     };
//
//     public message: string = "发送验证码";
//     public disabledCodeBtn: boolean = false;
//     $axios: any;
//     // 获取验证码
//     sendCode(ref: string) {
//         // 对email进行验证，验证通过发送验证码
//         (this.$refs[ref] as any).validateField(
//             ["email"],
//             (errorMessage: string) => {
//                 this.disabledCodeBtn = true; // 禁止按钮
//                 this.msgCode();
//                 this.$axios
//                     .post(
//                         "/get/code",
//                         new URLSearchParams({
//                             email: this.retrieveData.email!
//                         })
//                     )
//                     .then((res: ResType & any) => {
//                         if (res.flag) {
//                             this.$message.success(res.message);
//                         } else throw new Error(res.message);
//                     })
//                     .catch(err => {
//                         // console.log(err);
//                         this.disabledCodeBtn = false; // 按钮可以点击
//                         this.$message.error(err.message);
//                     });
//             }
//         );
//     }
//     public msgCode() {
//         let times: number; // 存储定时器
//         let s = 60;
//         this.message = `${s}秒`; // 先运行
//         s--;
//         times = setInterval(() => {
//             this.message = `${s}秒`;
//             s--;
//             if (s === 0) {
//                 clearInterval(times);
//                 this.message = "发送验证码";
//                 this.disabledCodeBtn = false;
//             }
//         }, 1000);
//     }
//     // 找回密码
//     public submitForm(ref: string) {
//         (this.$refs[ref] as any).validate((valid: boolean) => {
//             if (!valid) {
//                 return false; // 返回false，触发验证规则
//             }
//             // 发送找回密码请求等,然后发起mutation
//             this.$axios
//                 .put("/retrieve", this.retrieveData)
//                 .then((res: ResType & any) => {
//                     if (res.flag) {
//                         this.$message.success(res.message);
//                         this.$emit("retrieveSuccess");
//                     } else throw new Error(res.message);
//                 })
//                 .catch(this.catchErrMsg);
//         });
//     }
// }
</script>
<style scoped>
.retrieve {
    padding: 10px 60px 10px 20px;
}
</style>