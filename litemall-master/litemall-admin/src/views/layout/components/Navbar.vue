<template>
  <div class="navbar">
    <hamburger :toggle-click="toggleSideBar" :is-active="sidebar.opened" class="hamburger-container" />

    <breadcrumb class="breadcrumb-container" />

    <div class="right-menu">
      <template v-if="device!=='mobile'">
        <el-tooltip :content="$t('navbar.tooltip.full_screen')" effect="dark" placement="bottom">
          <screenfull class="right-menu-item" />
        </el-tooltip>

        <el-tooltip :content="$t('navbar.tooltip.size_select')" effect="dark" placement="bottom">
          <size-select class="right-menu-item" />
        </el-tooltip>

        <el-tooltip :content="$t('navbar.tooltip.notice')" effect="dark" placement="bottom">
          <notice class="right-menu-item" />
        </el-tooltip>
      </template>

      <el-dropdown class="avatar-container right-menu-item" trigger="click">
        <div class="avatar-wrapper">
          <img :src="avatar+'?imageView2/1/w/80/h/80'" class="user-avatar">
          <i class="el-icon-caret-bottom" />
        </div>
        <el-dropdown-menu slot="dropdown">
          <router-link to="/">
            <el-dropdown-item>
              {{ $t('navbar.menu.home') }}
            </el-dropdown-item>
          </router-link>
          
          <el-dropdown-item divided>
            <router-link to="/profile/password">
              {{ $t('navbar.menu.password') }}
            </router-link>
          </el-dropdown-item>
          <el-dropdown-item divided @click.native="logout">
            <span style="display:block;">{{ $t('navbar.menu.logout') }}</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Breadcrumb from '@/components/Breadcrumb'
import Hamburger from '@/components/Hamburger'
import Screenfull from '@/components/Screenfull'
import SizeSelect from '@/components/SizeSelect'
import Notice from '@/components/Notice'

export default {
  components: {
    Breadcrumb,
    Hamburger,
    Screenfull,
    SizeSelect,
    Notice
  },
  computed: {
    ...mapGetters([
      'sidebar',
      'name',
      'avatar',
      'device'
    ])
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch('toggleSideBar')
    },
    logout() {
      this.$store.dispatch('LogOut').then(() => {
        this.$router.push(`/login?redirect=${this.$route.fullPath}`)
      })
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.navbar {
  height: 50px;
  line-height: 50px;
  border-radius: 0px !important;
  .hamburger-container {
    line-height: 58px;
    height: 50px;
    float: left;
    padding: 0 10px;
  }
  .breadcrumb-container{
    float: left;
  }
  .errLog-container {
    display: inline-block;
    vertical-align: top;
  }
  .right-menu {
    float: right;
    height: 100%;
    &:focus{
     outline: none;
    }
    .right-menu-item {
      display: inline-block;
      margin: 0 8px;
      vertical-align: top;
    }
    .avatar-container {
      height: 50px;
      margin-right: 30px;
      .avatar-wrapper {
        cursor: pointer;
        margin-top: 5px;
        position: relative;
        .user-avatar {
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }
        .el-icon-caret-bottom {
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }
}
</style>
