<template>
  <div class="layout-main-group" :style="styleLayoutMainGroup">
    <div class="layout-main-mask"></div>
    <el-container class="layout-main">
      <!-- 顶栏 -->
      <el-header>
        <div class="logo-group" :style="{width: collapse ? asideWidthCollapse : asideWidth}">
          <img v-if="collapse" :src="`${$assetsPublicPath}static/image/theme/${themeName}/logo/icon-only.png`">
          <img v-else :src="`${$assetsPublicPath}static/image/theme/${themeName}/logo/all.png`">
        </div>
        <div class="toggle-aside-btn" @click="collapse = !collapse">
          <d2-icon name="bars"/>
        </div>
        <d2-layout-main-menu-header/>
        <!-- 顶栏右侧 -->
        <div class="d2-header-right">
          <d2-layout-main-header-help/>
          <d2-layout-main-header-full-screen/>
          <d2-layout-main-header-theme/>
          <d2-layout-main-header-user/>
        </div>
      </el-header>
      <!-- 下面 主体 -->
      <el-container>
        <!-- 主体 侧边栏 -->
        <el-aside ref="aside" :style="{width: collapse ? asideWidthCollapse : asideWidth}">
          <d2-layout-main-menu-side :collapse="collapse"/>
        </el-aside>
        <!-- 主体 -->
        <el-main>
          <transition name="fade-transverse">
            <router-view/>
          </transition>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import { mapState, mapMutations } from 'vuex'
export default {
  name: 'd2-layout-main',
  components: {
    'd2-layout-main-menu-side': () => import('./components/-menu-side'),
    'd2-layout-main-menu-header': () => import('./components/-menu-header'),
    'd2-layout-main-header-full-screen': () => import('./components/-full-screen'),
    'd2-layout-main-header-theme': () => import('./components/-theme'),
    'd2-layout-main-header-user': () => import('./components/-user'),
    'd2-layout-main-header-help': () => import('./components/-help')
  },
  data () {
    return {
      collapse: false,
      // [侧边栏宽度] 正常状态
      asideWidth: '200px',
      // [侧边栏宽度] 折叠状态
      asideWidthCollapse: '65px'
    }
  },
  computed: {
    ...mapState({
      themeName: state => state.theme.name,
      themeBackGroundImage: state => state.theme.backGroundImage
    }),
    styleLayoutMainGroup () {
      return {
        ...this.themeBackGroundImage ? {backgroundImage: `url('${this.$assetsPublicPath}${this.themeBackGroundImage}')`} : {}
      }
    }
  },
  mounted () {
    // 加载主题
    this.loadTheme()
  },
  methods: {
    ...mapMutations([
      'loadTheme'
    ])
  }
}
</script>

<style lang="scss">
// 注册主题
@import '~@/assets/style/theme/register.scss';
</style>
