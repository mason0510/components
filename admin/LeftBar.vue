<template>
    <el-scrollbar class="el-scrpllbar">
        <el-menu class="el-menu-slide" :default-active="$router.currentRoute.path" router>
            <template v-for="route in getRoutes">
                <!-- 单个元素的 -->
                <el-menu-item
                    :index="route.path"
                    :key="route.name"
                >
                    <i v-if="route.meta.icon" :class="route.meta.icon"></i>
                    <span slot="title">{{route.meta.title}}</span>
                </el-menu-item>
            </template>
        </el-menu>
    </el-scrollbar>
</template>

<script lang="ts">
// import { Component, Vue } from "vue-property-decorator";
// import { Getter } from "vuex-class";
// @Component({
//     components: {}
// })
// export default class LeftBar extends Vue {
//     @Getter("getRoutes") getRoutes: any;
// }
</script>

<style>
.el-scrollbar {
    height: 100%;
    border-right: 1px solid #e6e6e6;
    background: #fff;
}
.el-scrollbar .el-menu-slide {
    border-right: none;
}
.el-scrollbar .el-menu-slide i {
    margin-right: 5px;
    width: 24px;
    text-align: center;
    font-size: 18px;
}
</style>
