<template>
  <q-layout ref="main" view="hHr LpR lFf" reveal>
    <q-toolbar slot="header" class="toolbar bg-pink"
      :left-breakpoint="996">
      <q-btn flat @click="$refs.main.toggleLeft()">
        <q-icon name="menu" />
      </q-btn>
      <q-toolbar-title :padding="0">
        Bandori {{$t('toolbar.title')}} v{{master.constants && master.constants.resVer ? master.constants.resVer : '0.0.0.0'}}
      </q-toolbar-title>
    </q-toolbar>

    <div slot="left">
      <q-list no-border link inset-separator>
        <q-list-header>{{$t('left.title')}}</q-list-header>
        <q-side-link item :to="{path: '/index', exact: true}">
          <q-item-side icon="home" />
          <q-item-main :label="$t('left.home')" />
        </q-side-link>
        <q-side-link item to="/card">
          <q-item-side icon="picture_in_picture" />
          <q-item-main :label="$t('left.card')" />
        </q-side-link>
        <q-side-link item to="/music">
          <q-item-side icon="library_music" />
          <q-item-main :label="$t('left.music')" />
        </q-side-link>
        <q-side-link item to="/sfcs">
          <q-item-side icon="photo_library" />
          <q-item-main :label="$t('left.SFC')" />
        </q-side-link>
        <q-side-link item to="/about">
          <q-item-side icon="info" />
          <q-item-main :label="$t('left.about')" />
        </q-side-link>
        <q-item-separator />
        <q-list-header>{{$t('left.secTitle')}}</q-list-header>
        <q-item>
          <q-item-main label="v0.1.1" />
        </q-item>
      </q-list>
    </div>

    <!--
      Replace following "div" with
      "<router-view class="layout-view">" component
      if using subRoutes
    -->
    <div class="layout-view">
      <router-view class="layout-padding" />
      <q-btn
        v-back-to-top.animate="{offset: 500, duration: 200}"
        round
        color="teal-5"
        class="fixed-bottom-right"
        style="margin: 0 15px 15px 0"
      >
        <q-icon name="keyboard_arrow_up" />
      </q-btn>
    </div>
  </q-layout>
</template>

<i18n>
{
  "en": {
    "toolbar": {
      "title": "Database"
    },
    "left": {
      "title": "Toolbox",
      "home": "Home",
      "card": "Cards",
      "music": "Musics",
      "SFC": "Loading Cartoons",
      "about": "About",
      "secTitle": "Building info"
    }
  },
  "zh-CN": {
    "toolbar": {
      "title": "数据库"
    },
    "left": {
      "title": "工具箱",
      "home": "首页",
      "card": "卡牌",
      "music": "歌曲",
      "SFC": "加载界面漫画",
      "about": "关于我们",
      "secTitle": "版本"
    }
  },
  "zh-TW": {
    "toolbar": {
      "title": "數據庫"
    },
    "left": {
      "title": "工具箱",
      "home": "首頁",
      "card": "卡牌",
      "music": "歌曲",
      "SFC": "加載界面漫畫",
      "about": "關於我們",
      "secTitle": "版本"
    }
  }
}
</i18n>

<script>
import {
  Loading,
  QLayout,
  QListHeader,
  QList,
  QIcon,
  QSideLink,
  QItemSide,
  QItemMain,
  QBtn,
  QToolbar,
  QToolbarTitle,
  QItem,
  BackToTop,
  QItemSeparator
} from 'quasar'
import { mapActions, mapState } from 'vuex'

export default {
  components: {
    QLayout,
    QListHeader,
    QList,
    QIcon,
    QSideLink,
    QItemSide,
    QItemMain,
    QBtn,
    QToolbar,
    QToolbarTitle,
    QItem,
    QItemSeparator
  },
  directives: {
    BackToTop
  },
  computed: {
    ...mapState('DB', [
      'master',
      'getMasterDBStatus',
      'live2d',
      'getLive2DDBStatus'
    ])
  },
  watch: {
    getMasterDBStatus (newVal) {
      if (newVal === 1) {
        Loading.show({
          message: 'Loading Master Database...'
        })
      }
      else {
        Loading.hide()
      }
    },
    getLive2DDBStatus (newVal) {
      if (newVal === 1) {
        Loading.show({
          message: 'Loading Live2D Database...'
        })
      }
      else {
        Loading.hide()
      }
    }
  },
  methods: {
    ...mapActions('DB', [
      'getMasterDB',
      'getLive2D'
    ])
  },
  mounted () {
    this.$nextTick(() => {
      if (this.getMasterDBStatus !== 2) {
        this.getMasterDB()
          .then(() => this.getLive2D())
      }
    })
  }
}
</script>

<style lang="stylus">
.logo-container
  width 192px
  height 268px
  perspective 800px
  position absolute
  top 50%
  left 50%
  transform translateX(-50%) translateY(-50%)
.logo
  position absolute
  transform-style preserve-3d
</style>
