<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/heima.png" alt />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px':'200px' ">
        <!-- 隐藏侧边栏 -->
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu
          :default-active="activePath"
          :collapse="isCollapse"
          :collapse-transition="false"
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409eff"
          unique-opened
          router
        >
          <!-- 一级菜单 -->
          <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
            <!-- 一级菜单模板区域 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{item.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item
              @click="saveNavState( `/${subItem.path}`)"
              :index=" `/${subItem.path}` "
              v-for="subItem in item.children"
              :key="subItem.id"
            >
              <template slot="title">
                <!-- 图标 -->
                <i class="el-icon-menu"></i>
                <!-- 文本 -->
                <span>{{subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容主体区域 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menuList: [],
      iconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      isCollapse: false,
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList() {
      const { data: res } = await this.$http.get('/menus')
      // console.log(res);
      if (res.meta.status !== 200) return this.$message.error('获取用户列表失败')
      this.menuList = res.data
    },
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    saveNavState(activePath) {
      console.log(activePath)
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>
<style lang="less" scoped>
.home-container {
  height: 100%;

  .el-header {
    background-color: #373d41;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-left: 0;
    color: #fff;
    font-size: 20px;

    div {
      display: flex;
      align-items: center;

      img {
        margin-right: 15px;
      }
    }
  }
  .el-aside {
    background-color: #333744;

    .toggle-button {
      background-color: #4a5064;
      color: #fff;
      font-size: 10px;
      text-align: center;
      line-height: 24px;
      letter-spacing: 0.2em;
      cursor: pointer;
    }

    .el-menu {
      border-right: none;

      .iconfont {
        margin-right: 10px;
      }
    }
  }
  .el-main {
    background-color: #eaedf1;
  }
}
</style>