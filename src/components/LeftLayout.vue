<template>
  <!-- 最外层套一层 div 避免 gitHubUser 还未加载完时，右侧占满整个浏览器 -->
  <div>
    <div class="left-layout-container pc">
      <div class="user-info">
        <img v-if="gitHubUser" @click="home" :src="gitHubUser.avatar_url">
        <div v-if="gitHubUser" class="login-name">{{gitHubUser.name}}</div>
        <!-- <div v-if="gitHubUser" class="user-bio">{{gitHubUser.bio}}</div> -->
      </div>
      <div class="blog-login" v-if="accessToken == null">
        <a :href="loginLink">Login with GitHub</a>
      </div>
      <ul class="other-site">
        <li v-for="site in thirdPartySite" :key="site.url" @click="openThirdPartySite(site.url)">
          <img :src="site.img" alt="Visit my GitHub">
        </li>
      </ul>
      <ul class="left-menu">
        <router-link :class="isBlog ? 'selected-menu':''" tag="li" :to="{name: 'BlogList'}">
          <span>个人笔记</span>
        </router-link>
        <router-link :class="isWebNav ? 'selected-menu':''" tag="li" :to="{name: 'WebNav'}">
          <span>前端导航</span>
        </router-link>
        <li class="">
          <span>
            <a href="https://ye63.com">清新版</a>
          </span>

        </li>

      </ul>
      <span id='cnzz_stat_icon_1262532008'></span>
      <div v-if="showQQGroup" class="qq-group">
        <img src="/jy/static/img/subscription.jpg">
      </div>

      <div class="copyright">@ {{copyright}}</div>
    </div>
    <!-- 移动端 -->
    <div class="navbar">
      <nav>
        <!--左侧图标-->
        <a class="return" @click="toggleMenu()">
          <i class="icon-directory" />
        </a>
        <!--标题-->
        <h1 class="nav_title">
          <p>个人笔记</p>
        </h1>
        <!--右侧图标-->
        <!--           <div class="nav_r">
            <a href="javascript:;">
              <i class="icon-search" />
            </a>
          </div> -->
      </nav>
    </div>
    <div class="left-user">
      <transition name="slide-fade">
        <div class="left-layout-container" v-if="showMenu">
          <div class="user-info">
            <img v-if="gitHubUser" @click="home" :src="gitHubUser.avatar_url">
            <div v-if="gitHubUser" class="login-name">{{gitHubUser.name}}</div>
          </div>
          <ul class="other-site">
            <li v-for="site in thirdPartySite" :key="site.url" @click="openThirdPartySite(site.url)">
              <img :src="site.img">
            </li>
          </ul>
          <ul class="left-menu">
            <router-link :class="isBlog ? 'selected-menu':''" tag="li" :to="{name: 'BlogList'}" @click.native="toggleMenu()">
              <span>个人笔记</span>
            </router-link>
            <router-link :class="isWebNav ? 'selected-menu':''" tag="li" :to="{name: 'WebNav'}" @click.native="toggleMenu()">
              <span>前端导航</span>
            </router-link>
            <!-- <router-link :class="isAboutMe ? 'selected-menu':''" tag="li" :to="{name: 'AboutMe'}" @click.native="toggleMenu()"><span>关于我</span></router-link> -->
          </ul>
          <div class="copyright">@ {{copyright}}</div>
        </div>
      </transition>
      <transition name="fade">
        <div class="left-layout-bg" v-if="showMenu" @click="toggleMenu()"></div>
      </transition>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.slide-fade-enter-active {
  transition: all 0.5s ease;
}
.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter,
.slide-fade-leave-active {
  left: -15rem;
  opacity: 0;
}
.fade-enter-active {
  transition: all 0.5s ease;
}
.fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}
.fade-enter,
.fade-leave-active {
  opacity: 0;
}
</style>
<script>
import { mapGetters, mapActions } from 'vuex'

export default {
  data() {
    return {
      showMenu: false
    }
  },

  computed: {
    ...mapGetters([
      'copyright',
      'recordNumber',
      'gitHubUser',
      'showQQGroup',
      'thirdPartySite',
      'loginLink',
      'accessToken'
    ]),
    isBlog() {
      return this.$route.name && this.$route.name.startsWith('Blog')
    },
    isWebNav() {
      return this.$route.name === 'WebNav'
    },
    isAboutMe() {
      return this.$route.name === 'AboutMe'
    }
  },
  methods: {
    ...mapActions([
      'setAccessToken',
      'setTokenUser'
    ]),
    home() {
      this.$router.push('/')
    },
    openThirdPartySite(url) {
      window.open(url)
    },
    toggleMenu() {
      this.showMenu = !this.showMenu
    },

  }
}
</script>


 
       

