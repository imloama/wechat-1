<template>
  <div class="oper-menu">
    <el-dropdown placement="left">
      <span class="el-dropdown-link">
        <el-badge :value="10">
          <i class="oper-item el-icon-s-operation"></i>
        </el-badge>
      </span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item>
          <router-link to="/setting" class="aside-menu-link">
            个人设置
          </router-link>
        </el-dropdown-item>
        <el-dropdown-item>
          <el-badge
            :value="validateUnReadCount"
            :hidden="validateUnReadCount === 0"
          >
            <router-link to="/system" class="aside-menu-link">
              系统消息
            </router-link>
          </el-badge>
        </el-dropdown-item>
        <el-dropdown-item>
          <router-link to="/setting" class="aside-menu-link">
            主题设置
          </router-link>
        </el-dropdown-item>
        <el-dropdown-item>
          <router-link to="/setting" class="aside-menu-link">
            反馈
          </router-link>
        </el-dropdown-item>
        <el-dropdown-item>
          <a class="aside-menu-link" @click="logout">
            退出
          </a>
        </el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>
</template>

<script>
import { removeCookie } from '@/utils/token'
export default {
  computed: {
    userInfo() {
      return this.$store.state.user.userInfo
    },
    unreadNews() {
      return this.$store.state.news.unreadNews
    },
    validateUnReadCount() {
      const validateSysUser = (this.$store.state.app.sysUsers || []).find(item => item.code === '111111')
      const key = validateSysUser._id + '-' + this.userInfo._id
      return this.unreadNews[key]
    }
  },
  methods: {
    logout() {
      this.$router.replace('/login')
      this.$socket.emit('leave')
      removeCookie()
    }
  },
}
</script>

<style lang="scss">
.oper-menu {
  .menu-item {
    display: block;
  }
}
</style>
