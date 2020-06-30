<template>
    <div class="register">
        <el-form :model="registerData" :rules="rules" ref="registerData" label-width="100px">
            <el-form-item label="姓名" prop="name">
                <el-input v-model="registerData.name"></el-input>
            </el-form-item>
            <el-form-item label="邮箱" prop="email">
                <el-input type="email" v-model="registerData.email"></el-input>
            </el-form-item>
            <el-form-item label="手机号" prop="phone">
                <el-input type="number" v-model="registerData.phone"></el-input>
            </el-form-item>
            <el-form-item label="用户名" prop="username">
                <el-input v-model="registerData.username"></el-input>
            </el-form-item>
            <el-form-item :label="isUserSetting ? '新密码' : '密码'" prop="password">
                <el-input type="password" v-model="registerData.password"></el-input>
            </el-form-item>
            <el-form-item label="验证码" prop="code" v-if="!isUserSetting">
                <el-input v-model="registerData.code">
                    <template slot="append">
                        <el-button
                            style="color:#74562c"
                            @click="sendCode('registerData')"
                            :disabled="disabledCodeBtn"
                        >{{message}}</el-button>
                    </template>
                </el-input>
            </el-form-item>
            <el-form-item>
                <el-button
                    type="primary"
                    @click="submitForm('registerData')"
                >{{isUserSetting ? '修改信息' : '注 册'}}</el-button>
                <el-button @click="resetForm('registerData')">重 置</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script lang="ts">
// import { Component, Vue, Prop } from "vue-property-decorator";
// import { Mutation, Getter } from "vuex-class";
// import FormValidateMixin from "../mixin/formData";
// import { UserType } from "../types/UserType";
// import { ResType } from "../types/ResType";
// import { types } from "../store";
// import ErrorMessageMixin from "../mixin/errMsg";
//
// @Component({
//     mixins: [FormValidateMixin, ErrorMessageMixin]
// })
// export default class Register extends Vue {
//     @Mutation(types.SET_INFO_USER) setInfoUser!: (val: any) => void;
//     @Getter("getInfoUser")
//     public user!: UserType;
//     @Prop({
//         type: Boolean,
//         default: false
//     })
//     public isUserSetting!: boolean; // 获取修改自己的信息
//     public registerData: UserType = {
//         name: "",
//         email: "",
//         phone: "",
//         username: "",
//         password: "",
//         code: ""
//     };
//
//     public rules = {
//         name: [
//             { required: true, message: "请输入名字", trigger: "blur" },
//             {
//                 min: 2,
//                 max: 20,
//                 message: "请输入正确的名字长度",
//                 trigger: "blur"
//             }
//         ],
//         email: [
//             { required: true, message: "请输入邮箱", trigger: "blur" },
//             {
//                 type: "email",
//                 message: "请输入正确的邮箱地址",
//                 trigger: ["blur", "change"]
//             }
//         ],
//         phone: [
//             {
//                 required: true,
//                 message: "请输入手机号",
//                 trigger: ["blur", "change"]
//             },
//             {
//                 min: 10,
//                 max: 13,
//                 message: "请输入长度正确的手机号",
//                 trigger: ["blur", "change"]
//             }
//         ],
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
//     created() {
//         if (this.isUserSetting) {
//             this.registerData = {...this.user, password: ''};
//         }
//     }
//     public message: string = "发送验证码";
//     public disabledCodeBtn: boolean = false;
//     // 获取验证码
//     sendCode(ref: string) {
//         // 对email进行验证，验证通过发送验证码
//         (this.$refs[ref] as any).validateField(
//             ["email"],
//             (errorMessage: string) => {
//                 this.disabledCodeBtn = true; // 禁止按钮
//                 this.msgCode(); // 开始倒计时
//                 this.$axios
//                     .post(
//                         "/get/code",
//                         new URLSearchParams({
//                             email: this.registerData.email!
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
//                         this.$message.error("验证码发送失败，请检查你的网络！");
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
//     // 注册
//     public submitForm(ref: string) {
//         (this.$refs[ref] as any).validate((valid: boolean) => {
//             if (!valid) {
//                 return false; // 返回false，触发验证规则
//             }
//             if (this.isUserSetting) {
//                 this.$emit("updateInfoMessage", this.registerData);
//                 return;
//             }
//             // 发送注册请求等,然后发起mutation
//             this.$axios
//                 .post(
//                     `/register?code=${this.registerData.code}`,
//                     this.registerData
//                 )
//                 .then((res: ResType & any) => {
//                     if (res.flag) {
//                         // 成功怎么样
//                         res.data && this.setInfoUser(res.data);
//                         // 发送请求什么的
//                         this.$message.success(res.message);
//                         this.$emit("registerSuccess");
//                     } else throw new Error(res.message);
//                 })
//                 .catch(this.catchErrMsg);
//         });
//     }
// }
</script>
<style scoped>
.register {
    padding: 10px 60px 10px 20px;
}
</style>