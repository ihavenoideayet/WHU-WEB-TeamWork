<template>
  <div>
    <el-image :src="require('@/assets/logo.jpeg')" fit="fit" class="barImg" style="height: 50px; width : 200px"></el-image>
  <el-menu :default-active="this.$route.path" router mode="horizontal" class="bar">
<!--    <el-menu-item>-->
<!--      <a href="https://www.whu.edu.cn/"><el-image  :src="require('@/assets/logo.png')" fit="fit" style="height: 65px"></el-image></a>-->
<!--    </el-menu-item>-->
<!--    el-menu-item的index设为要跳转的路由（并且点击菜单项，会直接push到点击的页面)。-->
<!--    注意：一定要设置el-submenu的index属性（1，2，3…）。不然会出bug-->
    <el-menu-item v-for="(item,i) in navList" :key="i" :index="item.name" >
      <i :class="item.navItemIcon"></i>
      {{ item.navItem }}
    </el-menu-item >

    <div class="right_part">
    <el-button v-show="!  isLogin () " type="primary" @click="registerVisible = true">注册</el-button>
    <el-button v-show="!  isLogin () " type="primary" @click="loginVisible = true">登录</el-button>
<!--      用于注册登录的弹窗-->
    <el-dialog title="登录" :visible.sync="loginVisible" :before-close="handleClose">
        <login></login>
    </el-dialog>
    <el-dialog title="注册" :visible.sync="registerVisible" :before-close="handleClose">
        <register></register>
    </el-dialog>
<!--      用于用户选项的下拉列表-->
    <el-dropdown>
    <span class="el-dropdown-link" v-show="  isLogin () ">
      <i class="el-icon-user"></i>Customer_Name<i class="el-icon-arrow-down el-icon--right"></i>
    </span>
    <el-dropdown-menu slot="dropdown" >
      <el-dropdown-item @click.native="Setting">Setting</el-dropdown-item>
      <el-dropdown-item @click.native="Loginout">Log out</el-dropdown-item>
    </el-dropdown-menu>
    </el-dropdown>
    </div>
  </el-menu>
  </div>
</template>

<script>
import Login from '@/pages/stu/components/Register-Login/Login.vue'
import Register from '@/pages/stu/components/Register-Login/Register.vue'
// import vuex from 'vuex'
export default {
  name: 'NavBar',
  watch: {
    LoginCondition: 'isLogin'

  },
  methods: {
    // 弹窗退出函数
    isLogin () {
      let tem = !!localStorage.getItem('isLogin')
      console.log('aaa')
      if (tem === true) {
        console.log('aaa')
        this.LoginCondition = true
        return true
      } else {
        this.LoginCondition = false
        return false
      }
    },
    handleClose () {
      this.registerVisible = false
      this.loginVisible = false
      // var tem = localStorage.getItem('isLogin')
      // console.log(tem)
      // localStorage.removeItem('isLogin')
      // tem = localStorage.getItem('isLogin')
      // console.log(tem)
    },

    // 上方注册按钮函数
    register () {
      this.$router.push({path: '/Register'})
    },
    // 上方登录按钮函数
    login () {
      this.$router.push({path: '/Login'})
    },
    // isLogin () {
    //   // 是否为登录状态
    //   var tem = localStorage.getItem('isLogin')
    //   console.log(tem)
    //   // if (this.$store.state.status === 'success') {
    //   //   console.log('aaa')
    //   //   this.LogCondition = true
    //   //   return true
    //   // } else {
    //   //   return false
    //   // }
    // },
    Setting () {
      this.$router.push({path: '/Setting/Profile'})
    },
    Loginout () {
      localStorage.removeItem('isLogin')
      var tem = localStorage.getItem('isLogin')
      console.log(tem)
      this.LoginCondition = false
      location.reload()
    },
    setBarWidth () {
      this.barWidth = window.innerWidth > 1230 ? 80 + '%' : 100 + '%'
    }
  },
  components: {
    'login': Login,
    'register': Register
  },
  data () {
    return {
      navList: [
        {
          name: '/Home',
          navItem: '首页',
          navItemIcon: 'el-icon-s-home'
        },
        {
          name: '/Food',
          navItem: '饮食指南',
          navItemIcon: 'el-icon-trophy'

        },
        {
          name: '/Prevention',
          navItem: '疾病预防',
          navItemIcon: 'el-icon-s-promotion'
        },
        {
          name: '/Tools',
          navItem: '更多工具',
          navItemIcon: 'el-icon-s-flag'
        }
      ],
      LoginCondition: false, // use this to judge whether it have been login
      registerVisible: false,
      loginVisible: false
    }
  },
  mounted () {
    window.addEventListener('resize', this.setBarWidth, false)
  }
}
</script>

<style scoped>
  .barImg{
    height: 65px;
    float: left;
    margin-right: 2%
  }
  .right_part{
    float:right
  }
  .bar{
    float: left;
    width: 80%;
    min-width: 650px
  }
  @media screen and (max-width: 1230px) {
    .bar{
      float: left;
      width: 100%;
      min-width: 650px
    }
  }
</style>
