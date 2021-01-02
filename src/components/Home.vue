<template>
    <el-container class="homeContainer">
      <!-- 侧边栏 -->
        <el-aside :width="isCollapse ? '64px':'200px'">
            <div class="homeLogo">
              <img src="../assets/Homyit.png" alt="">
            </div>
            <el-menu
            :default-active="activePath"
            background-color="#344766"
            text-color="#fff"
            active-text-color="#ffd04b"
            unique-opened
            :collapse="isCollapse"
            :collapse-transition="false"
            router>
            <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
              <!-- 一级菜单 -->
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{item.authName}}</span>
              </template>
                <el-menu-item
                  :index="'/' + subItem.path + ''"
                  v-for="subItem in item.children"
                  :key="subItem.id"
                  @click="savaNavStatus('/' + subItem.path)">
                  <template slot="title">
                    <i class="el-icon-s-grid"></i>
                    <span>{{subItem.authName}}</span>
                  </template>
                </el-menu-item>
            </el-submenu>
          </el-menu>
        </el-aside>
        <el-container>
          <!-- 头部 -->
          <el-header>
          <div>
            <i :class="isCollapse ? 'el-icon-s-unfold':'el-icon-s-fold'" @click="toggleCollapse"></i>
          </div>
          <el-button type="primary" @click="loginOut">退出</el-button>
          </el-header>
          <!-- 主体 -->
          <el-main>
            <!-- 路由占位符 -->
            <router-view></router-view>
          </el-main>
        </el-container>
    </el-container>
</template>

<script>
export default {
  data () {
    return {
      menuList: [],
      // 菜单是否收缩
      isCollapse: false,
      // 被激活的链接地址
      activePath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.activePath
  },
  methods: {
    // 退出
    loginOut () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取菜单
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
      console.log(res)
    },
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    // 保存链接激活状态
    savaNavStatus (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}

</script>

<style lang='less' scoped>
.homeContainer {
  height: 100vh;
}

.el-header {
  display: flex;
  justify-content: space-between;
  background: #fff;
  align-items: center;
  > div{
    display: flex;
    align-items: center;
    span{
      margin-left: 15px;
      color: #000;
      font-size: 1.5rem;
    }
  }
}

.el-aside {
  background: #344766;
  .homeLogo {
    text-align: center;
    background: #344759;
    img {
      height: 40px;
      padding: 8px;
    }
  }
  .el-menu {
    border: none;
  }
}

.el-main {
  background: #eee;
}

.el-icon-s-fold,
.el-icon-s-unfold {
  display: block;
  padding: 10px;
  height: 100%;
  color: #409EFF;
  cursor: pointer;
  transform: scale(1.5);
}
</style>
