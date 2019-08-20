<template>
  <Layout style="height: 100%" class="main">
    <Menu mode="horizontal" theme="dark" active-key="1" class="css_menu_top">
      <div class="layout-logo">
        <Icon type="logo-instagram" size="32" />
      </div>
      <div class="layout-nav">
        <Submenu name="1">
          <template slot="title">
            <Icon type="ios-stats" />
            文件
          </template>
          <MenuItem name="1-1">新建</MenuItem>
          <MenuItem name="1-2">活跃分析</MenuItem>
          <MenuItem name="1-3">时段分析</MenuItem>
        </Submenu>
        <MenuItem key="2">
        导航二
        </MenuItem>
        <MenuItem key="3">
        导航三
        </MenuItem>
        <MenuItem key="4">
        导航四
        </MenuItem>
      </div>
    </Menu>
    <Layout>
      <Sider hide-trigger width="50">
        <Menu mode="horizontal" theme="dark" active-key="1" width="auto" :open-keys="['1']" class="css_menu_left">
          <MenuItem key="1">
          <Button icon="md-document" @click="files" ghost="true" type="text"></Button>
          </MenuItem>
          <MenuItem key="2">
          <Button icon="ios-keypad" @click="components" ghost="true" type="text"></Button>
          </MenuItem>
          <MenuItem key="3">
          <Button icon="ios-analytics" ghost="true" type="text"></Button>
          </MenuItem>
          <MenuItem key="4">
          <Button icon="ios-paper" ghost="true" type="text"></Button>
          </MenuItem>
        </Menu>
      </Sider>
      <Layout>
        <Split v-model="split1" min="300px" max="200px">
          <div slot="left" class="filelist-split-pane">
            <side-menu v-if="filesShow" accordion ref="sideMenu" :active-name="$route.name" :collapsed="collapsed" @on-select="turnToPage" :menu-list="menuList">
            </side-menu>
            <file-tree v-else>
              ss
            </file-tree>
          </div>
          <div slot="right" class="content-split-pane">
            <Drawingboard />
          </div>
        </Split>
      </Layout>
    </Layout>
  </Layout>
</template>
<script>
import SideMenu from './components/side-menu'
import FileTree from './components/file-tree'
import Drawingboard from './components/drawing-board'
import User from './components/user'
import ABackTop from './components/a-back-top'
import Fullscreen from './components/fullscreen'
import Language from './components/language'
import ErrorStore from './components/error-store'
import { mapMutations, mapActions, mapGetters } from 'vuex'
import { getNewTagList, routeEqual } from '@/libs/util'
import routers from '@/router/routers'
import minLogo from '@/assets/images/logo-min.jpg'
import maxLogo from '@/assets/images/logo.jpg'
import './mainboard.less'
export default {
  name: 'Mainboard',
  components: {
    SideMenu,
    FileTree,
    Drawingboard,
    Language,
    Fullscreen,
    ErrorStore,
    User,
    ABackTop
  },
  data () {
    return {
      collapsed: false,
      minLogo,
      maxLogo,
      isFullscreen: false,
      split1: 0.2,
      filesShow: false
    }
  },
  computed: {
    ...mapGetters([
      'errorCount'
    ]),
    tagNavList () {
      return this.$store.state.app.tagNavList
    },
    tagRouter () {
      return this.$store.state.app.tagRouter
    },
    userAvatar () {
      return this.$store.state.user.avatarImgPath
    },
    cacheList () {
      const list = ['ParentView', ...this.tagNavList.length ? this.tagNavList.filter(item => !(item.meta && item.meta.notCache)).map(item => item.name) : []]
      return list
    },
    menuList () {
      return this.$store.getters.menuList
    },
    local () {
      return this.$store.state.app.local
    },
    hasReadErrorPage () {
      return this.$store.state.app.hasReadErrorPage
    },
    unreadCount () {
      return this.$store.state.user.unreadCount
    }
  },
  methods: {
    ...mapMutations([
      'setBreadCrumb',
      'setTagNavList',
      'addTag',
      'setLocal',
      'setHomeRoute',
      'closeTag'
    ]),
    ...mapActions([
      'handleLogin',
      'getUnreadMessageCount'
    ]),
    components () {
      this.filesShow = false
    },
    files () {
      this.filesShow = true
    },
    handleCollapsedChange (state) {
      this.collapsed = state
    },
    handleCloseTag (res, type, route) {
      if (type !== 'others') {
        if (type === 'all') {
          this.turnToPage(this.$config.homeName)
        } else {
          if (routeEqual(this.$route, route)) {
            this.closeTag(route)
          }
        }
      }
      this.setTagNavList(res)
    },
    handleClick (item) {
      this.turnToPage(item)
    }
  },
  watch: {
    '$route' (newRoute) {
      const { name, query, params, meta } = newRoute
      this.addTag({
        route: { name, query, params, meta },
        type: 'push'
      })
      this.setBreadCrumb(newRoute)
      this.setTagNavList(getNewTagList(this.tagNavList, newRoute))
      this.$refs.sideMenu.updateOpenName(newRoute.name)
    }
  },
  mounted () {
    /**
     * @description 初始化设置面包屑导航和标签导航
     */
    this.setTagNavList()
    this.setHomeRoute(routers)
    const { name, params, query, meta } = this.$route
    this.addTag({
      route: { name, params, query, meta }
    })
    this.setBreadCrumb(this.$route)
    // 设置初始语言
    this.setLocal(this.$i18n.locale)
    // 如果当前打开页面不在标签栏中，跳到homeName页
    if (!this.tagNavList.find(item => item.name === this.$route.name)) {
      this.$router.push({
        name: this.$config.homeName
      })
    }
    // 获取未读消息条数
    this.getUnreadMessageCount()
  }
}
</script>
