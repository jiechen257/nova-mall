<template>
  <view>
    <view
      :class="{
        header_fixed: navFixed,
        header_absolute: type == 'transparent',
        header_shadow: navShadow,
        header_colorWhite: isWhite,
        themeBgColor: themeBgColor,
      }"
      :style="{
        paddingTop: statusBarHeight + 'px',
        backgroundImage: navBgColor,
        color: navFontColor,
        opacity: transparentValue,
      }"
    >
      <view class="header_content">
        <view class="header_left_box">
          <slot name="left">
            <view
              class="header_left_info"
              :class="{
                header_btnMongol: isTwoBtn,
                header_colorWhite_btnMongol: isWhite && isTwoBtn,
              }"
              v-if="back || $slots.left || home"
            >
              <view
                class="header_left_back"
                :class="{ header_btnMongol_left_back: isTwoBtn }"
                v-if="back && !firstPage"
                @click="onBackPage"
              >
                <image
                  class="header_icon"
                  v-if="isWhite"
                  src="/static/icon/nav-left.png"
                  mode="aspectFit"
                ></image>
                <image
                  class="header_icon"
                  v-else
                  src="/static/icon/nav-left.png"
                  mode="aspectFit"
                ></image>
              </view>
              <text
                class="header_left_line"
                :class="{ header_colorWhite_left_line: isWhite }"
                v-if="isTwoBtn"
              ></text>
              <view
                class="header_left_home"
                :class="{ header_btnMongol_left_home: isTwoBtn }"
                v-if="(firstPage && back) || home"
                @click="onBackHome"
              >
                <image
                  class="header_home_icon"
                  v-if="isWhite"
                  src="/static/tabBar/home.png"
                  mode="aspectFit"
                ></image>
                <image
                  class="header_home_icon"
                  v-else
                  src="/static/tabBar/home.png"
                  mode="aspectFit"
                ></image>
              </view>
            </view>
          </slot>
          <view
            class="header_title"
            v-if="!titleCenter && ($slots.default || navTitle)"
            :style="{ color: navFontColor }"
          >
            <slot
              ><text :style="{ color: navFontColor }">{{ navTitle }}</text></slot
            >
          </view>
        </view>
        <view
          class="header_title header_title_center"
          v-if="titleCenter && ($slots.default || navTitle)"
          :style="{ color: navFontColor }"
        >
          <slot
            ><text :style="{ color: navFontColor }">{{ navTitle }}</text></slot
          >
        </view>
        <view class="header_right_info">
          <slot name="right"></slot>
        </view>
      </view>
    </view>
    <view
      class="header_transparentFixed header_fixed"
      v-if="type == 'transparentFixed'"
      :style="{
        paddingTop: statusBarHeight + 'px',
        color: navTransparentFixedFontColor,
        opacity: 1 - transparentValue,
        zIndex: transparentValue < 0.3 ? 100 : 90,
      }"
    >
      <view class="header_content">
        <view class="header_left_box">
          <slot name="transparentFixedLeft">
            <view
              class="header_left_info header_transparentFixed_left_info"
              :class="{ header_transparentFixed_colorWhite_left_info: isWhite }"
              v-if="back || $slots.left || home"
            >
              <view
                class="header_left_back"
                :class="{ header_btnMongol_left_back: isTwoBtn }"
                v-if="back && !firstPage"
                @click="onBackPage"
              >
                <image
                  class="header_icon"
                  v-if="isWhite"
                  src="/static/icon/nav-left.png"
                  mode="aspectFit"
                ></image>
                <image
                  class="header_icon"
                  v-else
                  src="/static/icon/nav-left.png"
                  mode="aspectFit"
                ></image>
              </view>
              <text class="header_left_line" v-if="isTwoBtn"></text>
              <view
                class="header_left_home"
                :class="{ header_btnMongol_left_home: isTwoBtn }"
                v-if="(firstPage && back) || home"
                @click="onBackHome"
              >
                <image
                  class="header_home_icon"
                  v-if="isWhite"
                  src="/static/tabBar/home.png"
                  mode="aspectFit"
                ></image>
                <image
                  class="header_home_icon"
                  v-else
                  src="/static/tabBar/home.png"
                  mode="aspectFit"
                ></image>
              </view>
            </view>
          </slot>
          <view
            class="header_title"
            v-if="!titleCenter && (navTitle || $slots.transparentFixed)"
            :style="{ color: navTransparentFixedFontColor }"
          >
            <slot name="transparentFixed"
              ><text :style="{ color: navTransparentFixedFontColor }">{{ navTitle }}</text></slot
            >
          </view>
        </view>
        <view
          class="header_title header_title_center"
          v-if="titleCenter && (navTitle || $slots.transparentFixed)"
          :style="{ color: navTransparentFixedFontColor }"
        >
          <slot name="transparentFixed"
            ><text :style="{ color: navTransparentFixedFontColor }">{{ navTitle }}</text></slot
          >
        </view>
        <view class="header_right_info">
          <slot name="transparentFixedRight"></slot>
        </view>
      </view>
    </view>
    <view v-if="type == 'fixed'" :style="{ paddingTop: statusBarHeight + 'px' }">
      <view class="header_station"></view>
    </view>
  </view>
</template>
<script>
  // ???????????????????????????
  // ??????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????
  // ????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????
  const mainPagePath = ['/pages/tabBar/home/home']
  //?????????????????????
  const homePath = '/pages/tabBar/home/home'
  //???????????????
  const whiteList = ['#FFF', '#FFFFFF', 'white', 'rgb(255,255,255)', 'rgba(255,255,255,1)']
  export default {
    props: {
      //????????????????????????
      // 1000 ??????????????????
      // 2000 ?????????????????????
      // 3000 ??????????????????????????????????????????????????????????????????backClick??????
      backState: {
        default: function () {
          return 1000
        },
      },
      //??????????????????????????????
      home: {
        type: Boolean,
        default: function () {
          return false
        },
      },
      //??????????????????????????????
      bgColor: {
        type: [String, Array],
        default: function () {
          return '#FFFFFF'
        },
      },
      // ???????????????????????????
      bgColorAngle: {
        type: [String, Number],
        default: function () {
          return 90
        },
      },
      //?????????????????????????????????????????????????????????????????????????????????????????????????????????
      fontColor: {
        type: String,
        default: function () {
          return '#333'
        },
      },
      //??????????????????
      titleCenter: {
        type: Boolean,
        default: function () {
          return true
        },
      },
      //??????
      title: {
        type: String,
        default: function () {
          return ''
        },
      },
      //?????? fixed????????? ??????
      // ordinary ????????? ?????????
      // transparent ??????????????????
      //transparentFixed  ???????????????
      type: {
        type: String,
        default: function () {
          return 'fixed'
        },
      },
      //?????????????????????????????????
      transparentFixedFontColor: {
        type: String,
        default: function () {
          return '#000000'
        },
      },
      // ??????????????????????????????(????????????????????????)
      scrollTop: {
        type: Number,
        default: function () {
          return 0
        },
      },
      // ??????????????????
      shadow: {
        type: Boolean,
        default: function () {
          return true
        },
      },
    },
    data() {
      return {
        //????????????????????????????????????
        firstPage: false,
        //????????????
        transparentValue: 1,
        //??????
        navTitle: '',
        //?????????
        navFontColor: '#000000',
        //?????????
        navBgColor: 'none',
        //??????????????????
        navTransparentFixedFontColor: '#000000',
        // ????????????
        themeBgColor: false,
        // ???????????????
        statusBarHeight: 0,
        // ??????????????????????????????
        lastFrontColor: '',
      }
    },
    computed: {
      back() {
        return this.backState == 1000 || this.backState == 3000
      },
      //????????????
      navFixed() {
        if (this.type == 'transparentFixed' || this.type == 'fixed') {
          return true
        } else {
          return false
        }
      },
      //???????????????????????????
      navShadow() {
        if (this.bgColor && typeof this.bgColor == 'string') {
          return this.shadow && this.type !== 'transparent' && whiteList.includes(this.bgColor)
        } else {
          return false
        }
      },
      //?????????????????????????????????
      isWhite() {
        return whiteList.includes(this.navFontColor)
      },
      //??????????????????????????????
      isTwoBtn() {
        return (this.backState == 1000 || this.backState == 3000) && this.home && !this.firstPage
      },
    },
    watch: {
      title(val) {
        this.navTitle = val
      },
      fontColor(val) {
        this.navFontColor = val
        this.settingColor()
      },
      bgColor(val) {
        this.getNavBgColor(val)
      },
      transparentFixedFontColor(val) {
        this.navTransparentFixedFontColor = val
      },
      scrollTop(val) {
        this.pageScroll({
          scrollTop: val,
        })
      },
    },
    //???????????????
    created() {
      this.navTitle = this.title
      this.navFontColor = this.fontColor
      this.getNavBgColor(this.bgColor)
      this.navTransparentFixedFontColor = this.transparentFixedFontColor
      //???????????????????????????
      this.statusBarHeight = uni.getSystemInfoSync()['statusBarHeight']
      const _this = this
      this.pageScroll({
        scrollTop: this.scrollTop,
      })
      //??????????????????
      let currentPages = getCurrentPages()
      let pageLen = currentPages.length
      //???????????????????????????????????????????????????back???true??????????????????????????????????????????????????????????????????
      if (pageLen == 1 && !mainPagePath.includes(currentPages[0].route)) {
        this.firstPage = true
      }
    },
    //??????
    methods: {
      //??????????????????
      onBackPage() {
        if (this.backState == 3000) {
          this.$emit('backClick')
        } else {
          uni.navigateBack()
        }
      },
      //????????????
      onBackHome() {
        uni.switchTab({
          url: homePath,
        })
      },
      pageScroll(e) {
        if (this.type == 'transparentFixed') {
          if (e.scrollTop && e.scrollTop > 0) {
            if (e.scrollTop > 180) {
              this.transparentValue = 1
            } else {
              this.transparentValue = e.scrollTop / 180
            }
          } else {
            this.transparentValue = 0
          }
          this.settingColor()
        }
      },
      // ????????????????????????
      getNavBgColor(val) {
        if (val == 'themeBgColor') {
          this.themeBgColor = true
          this.navBgColor = 'none'
        } else if (this.type == 'transparent') {
          this.themeBgColor = false
          this.navBgColor = 'none'
        } else {
          if (typeof val == 'string') {
            this.navBgColor = 'linear-gradient(90deg,' + val + ',' + val + ')'
          } else if (Array.isArray(val) && val.length >= 2) {
            let navBgColor = 'linear-gradient(' + this.bgColorAngle + 'deg'
            val.forEach((item) => {
              if (typeof item == 'string') {
                navBgColor += ',' + item
              } else if (typeof item == 'object') {
                navBgColor += ',' + item.color + ' ' + item.scale
              }
            })
            navBgColor += ')'
            this.navBgColor = navBgColor
          }
        }
      },
      //???????????????????????????
      settingColor() {
        let navColor = this.navFontColor
        if (this.type == 'transparentFixed' && this.transparentValue <= 0.5) {
          navColor = this.navTransparentFixedFontColor
        }
        let frontColor = '#000000'
        if (whiteList.includes(navColor)) {
          frontColor = '#ffffff'
        }
        if (this.lastFrontColor == frontColor) {
          return
        }
        setTimeout(() => {
          this.lastFrontColor = frontColor
          // ???????????????????????????
          uni.setNavigationBarColor({
            frontColor: frontColor,
            backgroundColor: '#FFFFFF',
          })
        }, 150)
      },
    },
  }
</script>
<style lang="scss">
  /* #ifdef APP-PLUS-NVUE */
  @function unit($num) {
    @return $num + 0rpx;
  }
  /* #endif */
  /* #ifndef APP-PLUS-NVUE */
  @function unit($num) {
    @return $num + 0upx;
  }
  /* #endif */
  .header_content {
    /* #ifndef APP-NVUE */
    display: flex;
    /* #endif */
    /* #ifdef MP */
    padding-right: unit(190);
    box-sizing: border-box;
    /* #endif */
    width: unit(750);
    align-items: flex-end;
    justify-content: space-between;
    flex-direction: row;
    height: unit(88);
    position: relative;
  }

  .header_station {
    height: unit(88);
  }

  .header_shadow {
    // border-style: solid;
    // border-width: unit(2);
    // border-color: #f5f5f5;
    // box-shadow: 0 0 unit(6) 0 #ddd;
    box-shadow: 0rpx 4rpx 8rpx 0rpx rgba(51, 51, 51, 0.08);
  }

  .header_fixed {
    position: fixed;
    top: 0;
    left: 0;
    width: unit(750);
    z-index: 99;
  }

  .header_absolute {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 99;
    width: unit(750);
    background-color: transparent !important;
  }

  .header_left_box {
    /* #ifndef APP-PLUS-NVUE */
    display: flex;
    /* #endif */
    flex-direction: row;
    align-items: center;
    height: unit(88);
    flex: 1;
  }

  .header_left_line {
    height: unit(30);
    width: unit(2);
    background-color: rgba(255, 255, 255, 0.4);
  }

  .header_left_back {
    width: unit(56);
    height: 100%;
    /* #ifndef APP-PLUS-NVUE */
    display: flex;
    /* #endif */
    align-items: center;
    justify-content: center;
  }

  .header_icon {
    width: unit(34);
    height: unit(32);
  }
  .header_home_icon {
    width: unit(34);
    height: unit(34);
  }

  .header_left_home {
    width: unit(56);
    height: 100%;
    /* #ifndef APP-PLUS-NVUE */
    display: flex;
    /* #endif */
    align-items: center;
    justify-content: center;
  }

  .header_left_info {
    /* #ifndef APP-PLUS-NVUE */
    display: flex;
    /* #endif */
    flex-direction: row;
    align-items: center;
    height: unit(56);
    margin-left: unit(16);
  }

  .header_title {
    height: unit(88);
    font-size: unit(32);
    padding-left: unit(30);
    padding-right: unit(30);
    font-weight: 700;
    text-overflow: ellipsis;
    /* #ifndef APP-PLUS-NVUE */
    white-space: nowrap;
    display: flex;
    overflow: hidden;
    /* #endif */
    /* #ifdef APP-PLUS-NVUE */
    lines: 1;
    /* #endif */
    flex-direction: row;
    align-items: center;
    justify-content: center;
    /* #ifdef MP */
    max-width: calc(100vw - 160upx);
    /* #endif */
  }

  .header_title_center {
    position: absolute;
    bottom: unit(0);
    left: unit(375);
    transform: translateX(-50%);
  }

  .header_right_info {
    height: unit(88);
    /* #ifndef APP-PLUS-NVUE */
    display: flex;
    flex-shrink: 0;
    /* #endif */
    flex-direction: row;
    align-items: center;
  }

  .header_btnMongol {
    border-radius: unit(33);
    border-style: solid;
    border-width: unit(2);
    border-color: rgba(0, 0, 0, 0.1);
    background-color: rgba(255, 255, 255, 0.7);
    /* #ifndef APP-PLUS-NVUE */
    box-sizing: border-box;
    /* #endif */
  }

  .header_btnMongol_left_back,
  .header_btnMongol_left_home {
    width: unit(70);
  }

  .header_transparentFixed {
    border-bottom-width: 0;
    background-color: transparent;
    background-image: transparent;
  }

  .header_transparentFixed_left_info {
    border-style: solid;
    border-width: unit(2);
    border-color: rgba(0, 0, 0, 0.1);
    background-color: rgba(255, 255, 255, 0.7);
    border-radius: unit(33);
    /* #ifndef APP-PLUS-NVUE */
    box-sizing: border-box;
    /* #endif */
  }

  .header_transparentFixed_colorWhite_left_info {
    border-style: solid;
    border-width: unit(2);
    border-color: rgba(255, 255, 255, 0.3);
    background-color: rgba(0, 0, 0, 0.2);
  }

  //????????????
  .header_colorWhite_btnMongol {
    border-style: solid;
    border-width: unit(2);
    border-color: rgba(255, 255, 255, 0.3);
    background-color: rgba(0, 0, 0, 0.2);
  }

  .header_colorWhite_left_line {
    background-color: rgba(255, 255, 255, 0.3);
  }
</style>
