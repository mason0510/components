<template>
    <div>
        <div class="current-date">
            <span>场地开放时间&nbsp;</span>&nbsp;&nbsp;&nbsp;
            <el-date-picker
                disabled
                :value="[areaData.areaStartTime,areaData.areaEndTime]"
                type="datetimerange"
                range-separator="至"
                start-placeholder="开始日期"
                end-placeholder="结束日期"
            ></el-date-picker>
        </div>
        <el-divider></el-divider>
        <el-form
            :model="bespeakData"
            :rules="rules"
            ref="bespeakData"
            label-width="120px"
            class="bespeak-form"
        >
            <el-form-item label="场地预约时间" required>
                <el-col :span="11">
                    <el-form-item prop="bespeakStartDate">
                        <el-date-picker
                            type="datetime"
                            placeholder="选择日期"
                            v-model="bespeakData.bespeakStartDate"
                            style="width: 100%;"
                        ></el-date-picker>
                    </el-form-item>
                </el-col>
                <el-col class="line" :span="2">-</el-col>
                <el-col :span="11">
                    <el-form-item prop="bespeakEndDate">
                        <el-date-picker
                            type="datetime"
                            placeholder="选择时间"
                            v-model="bespeakData.bespeakEndDate"
                            style="width: 100%;"
                        ></el-date-picker>
                    </el-form-item>
                </el-col>
            </el-form-item>
            <el-form-item>
                <el-button
                    type="primary"
                    @click="submitForm('bespeakData')"
                    :disabled="isBespeak"
                >立即预约</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
import { AreaType } from "../types/AreaType";
import { BespeakType } from "../types/BespeakType";

@Component
export default class BespeakForm extends Vue {
    // prop -> areaData ,type is AreaType
    @Prop(Object) areaData!: AreaType;
    public bespeakData: any = {
        areaId: "",
        bespeakStartDate: new Date(),
        bespeakEndDate: new Date()
    };
    public isBespeak: boolean = false;
    // created() {
    //     this.initBespeak();
    // }
    @Watch("areaData", {
        immediate: true
    })
    updateAreaData() {
        this.initBespeak();
    }
    public initBespeak() {
        try {
            this.isBespeak = false;
            const { areaId, areaStartTime, areaEndTime } = this.areaData;
            this.bespeakData = {
                areaId: areaId!,
                bespeakStartDate: areaStartTime,
                bespeakEndDate: areaEndTime
            };
        } catch (error) {}
    }
    public rules = {
        bespeakStartDate: [
            {
                type: "date",
                required: true,
                message: "请选择日期",
                trigger: "change"
            },
            {
                validator: (rule: any, value: any, callback: any) => {
                    const currentDate = new Date(
                        this.areaData.areaStartTime
                    ).getTime();
                    const endDate = new Date(
                        this.areaData.areaEndTime
                    ).getTime();
                    const date = new Date(value).getTime();
                    // console.log(currentDate, date);
                    if (currentDate <= date && endDate > date) {
                        callback();
                    } else {
                        callback(new Error("开始时间请在场地开放时间之内！"));
                    }
                },
                trigger: "blur"
            }
        ],
        bespeakEndDate: [
            {
                type: "date",
                required: true,
                message: "请选择时间",
                trigger: "change"
            },
            {
                validator: (rule: any, value: any, callback: any) => {
                    const currentDate = new Date(
                        this.areaData.areaEndTime
                    ).getTime();
                    const startDate = new Date(
                        this.areaData.areaStartTime
                    ).getTime();
                    const date = new Date(value).getTime();
                    // console.log(currentDate, date);
                    if (currentDate >= date && startDate < date) {
                        callback();
                    } else {
                        callback(new Error("选择时间不能比开放时间后！"));
                    }
                },
                trigger: "blur"
            }
        ]
    };
    public submitForm(ref: string) {
        (this.$refs[ref] as any).validate((valid: boolean) => {
            if (!valid) return false;
            const { bespeakStartDate, bespeakEndDate } = this.bespeakData;
            if (
                new Date(bespeakStartDate).getTime() >=
                new Date(bespeakEndDate).getTime()
            ) {
                this.$message.error("开始时间不能大于结束时间！！！");
                return;
            }
            this.isBespeak = true;
            this.$emit("addBespeak", this.bespeakData);
        });
    }
}
</script>

<style>
.bespeak-form .line {
    text-align: center;
}
.current-date {
    padding: 0 30px;
}
</style>