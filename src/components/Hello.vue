<template>
    <div class="screen-container">
      <div class="screen-header">
        <div class="logo">
          <img :src="logo" class="logo-img" alt="logo">
        </div>
        <div class="title">
          <div class="company-message"  @click="fullScreen()">
            <h2>车势科技</h2>
            <h3 class="company-sub-title">汽车行业智能销售专家</h3>
          </div>
          <div class="scroll-message">
            <div class="current-view scroll-style">
              {{scrollText}}
            </div>
            <div class="current-lookcar scroll-style scroll-swiper">
              <swiper :options="carSwiperOption" ref="carSwiper">
                <swiper-slide
                  v-for="car in carList"
                  key="car.car_id"
                >
                  <span class="slide-inner-text">{{ car.carname }}</span>
                </swiper-slide>
                <div class="swiper-button-prev swiper-button-red" slot="button-prev"></div>
                <div class="swiper-button-next swiper-button-red" slot="button-next"></div>
              </swiper>
            </div>
            <div class="current-day scroll-style scroll-swiper">
              <swiper :options="dateSwiperOption" ref="dateSwiper">
                <swiper-slide
                  v-for="date in dateList"
                  key="date"
                >
                  <span class="slide-inner-text">{{ date }}</span>
                </swiper-slide>
                <div class="swiper-button-prev swiper-button-red" slot="button-prev"></div>
                <div class="swiper-button-next swiper-button-red" slot="button-next"></div>
              </swiper>
            </div>
          </div>
        </div>
      </div>
      <div class="content">
        <div class="content-left">
          <div class="overview core-mr">
            <div class="overview-title">
              <div class="overview-title-bg">
                <img :src="overviewBg" class="overview-img" alt="overview">
              </div>
              <div class="overview-title-name">
                <p class="main-title">信息总览</p>
                <p class="sub-title">当天客户数据</p>
              </div>
            </div>
            <div class="overview-vr-experience core-mb overview-four-part">
              <p class="info-number info-mb">
                <i-odometer
                  class="iOdometer"
                  :value="totalUsers"
                  :duration="2500"
                ></i-odometer>
              </p>
              <p class="divide-line divide-line-red info-mb"></p>
              <p class="info-title">移动VR体验总人数</p>
            </div>
            <div class="overview-online core-mb overview-four-part">
              <p class="info-number info-mb">
                <i-odometer
                  :value="currentOnlineUsers"
                ></i-odometer>
              </p>
              <p class="divide-line divide-line-yellow info-mb"></p>
              <p class="info-title">当前在线人数</p>
            </div>
            <div class="ave-look core-mb overview-four-part">
              <p class="info-number info-mb">
                <i-odometer
                  :value="averageLookTime"

                ></i-odometer>
              </p>
              <p class="divide-line divide-line-purple info-mb"></p>
              <p class="info-title">平均看车时间（分钟）</p>
            </div>
            <div class="overview-sex overview-four-part">
              <p class="info-number info-mb info-number-sex">
                <span class="sex-boy sex">
                  <img class="man sex-icon" :src="man" alt="man">
                  <i-odometer
                    :value="mrate"
                  ></i-odometer>
                  %
                </span>
                <span class="sex-girl sex">
                  <img class="girl sex-icon" :src="girl" alt="girl">
                  <i-odometer
                    :value="wrate"
                  ></i-odometer>
                  %
                </span>
              </p>
              <p class="divide-line divide-line-green info-mb"></p>
              <p class="info-title">男女占比</p>
            </div>
          </div>
          <div class="core-mid core-mr">
            <div class="color-purchase-wrapper core-mb">
              <div class="core-map core-mr purchase">
                <div class="core-header">
                  <div class="core-header-icon">
                    <img :src="like" alt="purchase">
                  </div>
                  <div class="core-header-title">
                    <p class="main-title">购买意向统计</p>
                    <p class="sub-title">客户购买意向人数</p>
                  </div>
                </div>
                <div class="core-content color-purchase-content">
                  <purchase :perVw="perVw" :listData="purchaseList" height='100%' width='100%' />
                </div>
              </div>
              <div class="core-map color-preference">
                <div class="core-header">
                  <div class="core-header-icon">
                    <img :src="color" alt="color">
                  </div>
                  <div class="core-header-title">
                    <p class="main-title">颜色偏好统计</p>
                    <p class="sub-title">看车颜色时长占比</p>
                  </div>
                </div>
                <div class="core-content color-purchase-content core-color">
                  <div class="core-color-item" v-for="(color, key) in carColorList">
                    <div class="core-color-item-left">
                      <span class="color-bg" :style="{ background: color[2] }">
                        <i class="color-bg-arrow"></i>
                      </span>
                      <span class="color-name">{{color[1]}}</span>
                    </div>
                    <div class="core-color-item-right">
                      <span class="color-rate-num">{{color[0]}}%</span>
                      <div class="color-chart">
                        <carColor :perVw="perVw" :colorIndex="key" :id="'car-preference-chart' + key" :listData="carColorList" height='100%' width='100%' />
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="today-look core-map">
              <div class="core-header">
                <div class="core-header-icon">
                  <img :src="todayBg" alt="today">
                </div>
                <div class="core-header-title">
                  <p class="main-title">当日看车人数</p>
                  <p class="sub-title">当日看车人数增长趋势</p>
                </div>
              </div>
              <div class="core-content core-content-btm">
                <active-user :perVw="perVw" :listData="activeUser" height='100%' width='100%' />
              </div>
            </div>
          </div>
          <div class="core-right">
            <div class="individuation core-map core-mb">
              <div class="core-header">
                <div class="core-header-icon">
                  <img :src="individuation" alt="individuation">
                </div>
                <div class="core-header-title">
                  <p class="main-title">个性化选配统计</p>
                  <p class="sub-title">观看总时长和次数</p>
                </div>
              </div>
              <div class="core-content core-content-top core-individuation">
                <div
                  class="individuation-item"
                  v-for="(item, key) in individuationList"
                >
                  <div class="individuation-img">
                    <img :src="item.src">
                  </div>
                  <div class="individuation-overview">
                    <p class="individuation-overview-title">
                      {{item.part_name}}
                    </p>
                    <p class="individuation-overview-looktime individuation-num">
                      <span>{{item.look_times}}</span>
                      <span title="观看总时长(分钟)">观看总时长(分钟)</span>
                    </p>
                    <p class="individuation-overview-lookcount individuation-num">
                      <span>{{item.time}}</span>
                      <span title="观看总次数">观看总次数</span>
                    </p>
                  </div>
                </div>
              </div>
            </div>
            <div class="purchase-color-chart-wrapper">
              <div class="core-header">
                <div class="core-header-icon">
                  <img :src="heat" class="heat-img" alt="heat">
                </div>
                <div class="core-header-title">
                  <p class="main-title">客户关注点热力图</p>
                  <p class="sub-title">看车时长热力图</p>
                </div>
              </div>
              <div class="core-content core-content-btm core-heat">
                <div class="core-heat-half core-heat-scatter" ref="scatter">
                  <img class="car" :src="car" alt="car">
                  <div class="focus-scatter" :style="{ width: scatterWidth }">
                    <focusScatter :lastTenLook="lastTenLook" :perVw="perVw" :listData="carfoucus" height='100%' :width='scatterWidth' />
                  </div>
                </div>
                <div class="core-heat-half core-heat-normal">
                  <focus :perVw="perVw" :listData="carfoucus" height='100%' width='100%' />
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="content-right">
          <div class="online-offline">
            <div class="online-num">
              <div class="online-top onoff-top">
                <span class="online-status">

                </span>
                <span class="online-count">
                  {{onlineCount}}<span class="people-unit">人</span>
                </span>
              </div>
              <div class="online-btm onoff-title">
                当前在线
              </div>
            </div>
            <div class="offline-num">
              <div class="offline-top onoff-top">
                <span class="offline-status">

                </span>
                <span class="offline-count">
                  {{offlineCount}}<span class="people-unit">人</span>
                </span>
              </div>
              <div class="offline-btm onoff-title">
                当日离线
              </div>
            </div>
          </div>
          <div class="current-online">

          </div>
        </div>
        <div class="current-user">
          <swiper :options="swiperOption" ref="mySwiper">
            <swiper-slide
              v-for="user in currentUsers"
              key="user.id"
              :data-uid="user.id"
            >
              <div class="single-user" @click="showDetailUserInfo(user, $event)">
                <p class="single-user-name-box" :style="{background: user.iconBg}">
                  <span class="single-user-name">{{ user.name }}</span>
                </p>
                <i class="arrow left-arrow"></i>
              </div>
            </swiper-slide>
          </swiper>
        </div>
      </div>
      <div class="user-detail" ref="userDetail" v-show="showUserOverview">
        <div class="user-detail-header user-detail-line">
          <div class="user-icon" :style="{background: userDetailHeaderBg}">
            <span>{{summary.username}}</span>
          </div>
          <div class="user-username">
            {{summary.username}}
          </div>
          <div class="user-phone">
            <img :src="summary.sex === '男' ? popMan : popGirl" class="user-sex user-phone-gap">
            <span class="user-age user-phone-gap">{{userAge}}</span>
            <span class="user-phone-number user-phone-gap">{{summary.phone}}</span>
          </div>
          <div class="user-tags">
            <span v-for="feature in summary.features" :title="feature">{{feature}}</span>
          </div>
        </div>
        <div class="user-detail-phone user-detail-line">
          <div class="phone-left phone-wrapper">
            <div class="phone-left-top phone-wrapper-inner phone-wrapper-inner-top">
              <div class="phone-wrapper-icon phone-wrapper-box">
                <img :src="popLike">
                <span>购买意向</span>
              </div>
              <div class="phone-wrapper-num phone-wrapper-box">
                <span class="phone-wrapper-placeholder"></span>
                <span class="phone-count purchase-count">{{summary.intention}}%</span>
              </div>
            </div>
            <div class="phone-left-top phone-wrapper-inner phone-wrapper-inner-btm">
              <div class="phone-wrapper-icon phone-wrapper-box">
                <img :src="popTel">
                <span>在线通话</span>
              </div>
              <div class="phone-wrapper-num phone-wrapper-box">
                <span class="phone-wrapper-placeholder"></span>
                <span class="phone-count">{{communicate}}</span>
              </div>
            </div>
          </div>
          <div class="phone-right">
            <div class="phone-left-top phone-wrapper-inner phone-wrapper-inner-top focus-car-outer">
              <div class="phone-wrapper-icon phone-wrapper-box">
                <img :src="popCar">
                <span>关注车系</span>
              </div>
              <div class="phone-wrapper-num phone-wrapper-box">
                <span class="phone-wrapper-placeholder"></span>
                <span :class="'focus-car' + summary.id" class="phone-count focus-car" :title="summary.lookcar">{{summary.lookcar}}</span>
              </div>
            </div>
            <div class="phone-left-top phone-wrapper-inner phone-wrapper-inner-btm">
              <div class="phone-wrapper-icon phone-wrapper-box">
                <img :src="popMessage">
                <span>语音留言</span>
              </div>
              <div class="phone-wrapper-num phone-wrapper-box">
                <span class="phone-wrapper-placeholder"></span>
                <span class="phone-count">{{voiceTime}}</span>
              </div>
            </div>
          </div>
        </div>
        <div class="user-detail-individuation user-detail-line">
          <div class="user-detail-individuation-top focus-title">
            <img :src="popEye">
            <span class="focus-title-name">{{userDetailIndividuaName}}</span>
          </div>
          <div class="user-detail-individuation-btm" v-if="summary.online">
            <div class="current-look-individuation" v-if="noCurrentLookIndividuation">
              暂无正在关注
            </div>
            <div class="current-has-look-individuation" v-else>
              <div class="focus-individuation-item" v-if="currentLookWheelhub">
                <img :src="onlineHub">
                <span :title="currentLookWheelhub">{{currentLookWheelhub}}</span>
                <i class="arrow-left-top"></i>
                <i class="arrow-right-btm"></i>
              </div>
              <div class="focus-individuation-item" v-if="currentLookGlass">
                <img :src="onlineGlass">
                <span :title="currentLookGlass">{{currentLookGlass}}</span>
                <i class="arrow-left-top"></i>
                <i class="arrow-right-btm"></i>
              </div>
              <div class="focus-individuation-item" v-if="currentLookColor.name">
                <div class="color-wrapper">
                  <span class="look-color" :style="{background: currentLookColor.value}">
                    <i class="look-color-arrow" :style="{background: currentLookColor.value}"></i>
                  </span>
                </div>
                <span :title="'车色-' + currentLookColor.name">车色-{{currentLookColor.name}}</span>
                <i class="arrow-left-top"></i>
                <i class="arrow-right-btm"></i>
              </div>
            </div>
          </div>
          <div class="user-detail-individuation-btm" v-else>
            <div class="focus-individuation-item" v-if="summary.wheel">
              <img :src="offlineHub">
              <span :title="summary.wheel">{{summary.wheel}}</span>
              <i class="arrow-left-top"></i>
              <i class="arrow-right-btm"></i>
            </div>
            <div class="focus-individuation-item" v-if="summary.glass">
              <img :src="offlineGlass">
              <span :title="summary.glass">{{summary.glass}}</span>
              <i class="arrow-left-top"></i>
              <i class="arrow-right-btm"></i>
            </div>
            <div class="focus-individuation-item" v-if="summary.color">
              <div class="color-wrapper">
                <span class="look-color" :style="{background: summary.color.value}">
                  <i class="look-color-arrow" :style="{background: summary.color.value}"></i>
                </span>
              </div>
              <span :title="'车色-' + summary.color.name">车色-{{summary.color.name}}</span>
              <i class="arrow-left-top"></i>
              <i class="arrow-right-btm"></i>
            </div>
          </div>
        </div>
        <div class="user-detail-fouce">
          <div class="user-detail-individuation-top focus-title focus-part-title">
            <img :src="popEye">
            <span class="focus-title-name">{{userDetailCarBodyName}}</span>
          </div>
          <div class="focus-part" v-if="summary.online">
            <div class="focus-now-box">
              <img :src="popCarbg" class="focus-now-icon">
              <div class="focus-now-name-wrapper">
                <span class="focus-now-name">
                  {{currentOnlineBodyLook}}
                </span>
                <span class="focus-now-title" v-if="currentOnlineBodyLook !== '暂未关注'">
                  客户正在观看
                </span>
              </div>
            </div>
          </div>
          <div class="focus-part" v-else>
            <span v-for="focus in summary.focus">{{focus}}</span>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import IOdometer from './iOdometer';
    import 'odometer/themes/odometer-theme-car.css';
    import 'odometer/themes/odometer-theme-digital.css';
    import './liMarquee.css';
    import './jquery.liMarquee.js';
    import focus from './focus';
    import focusScatter from './focusScatter';
    import purchase from './purchase';
    import activeUser from './activeUser';
    import carColor from './carColor';
    import overviewBg from './overviewBg.svg';
    import logo from './logo.svg';
    import man from './man.svg';
    import girl from './girl.svg';
    import heat from './heat.svg';
    import todayBg from './todayBg.svg';
    import individuation from './individuation.svg';
    import like from './like.svg';
    import color from './color.svg';
    import car from './car.svg';
    import popGirl from './pop-girl.svg';
    import popMan from './pop-man.svg';
    import hub18 from './hub18.jpg';
    import hub19 from './hub19.jpg';
    import glass_no_privacy from './glass_no_privacy.jpg';
    import glass_has_privacy from './glass_has_privacy.jpg';
    import popLike from './pop-like.svg';
    import popCar from './pop-car.svg';
    import popTel from './pop-tel.svg';
    import popMessage from './pop-message.svg';
    import popEye from './pop-eye.svg';
    import popCarbg from './pop-carbg.svg';


    axios.defaults.headers.common['Content-type'] = "application/json";

    export default {
      name: 'Screen',
      components: {
        focus,
        purchase,
        activeUser,
        carColor,
        focusScatter,
        IOdometer,
      },
      data() {
        const $this = this;
        return {
          scrollTextList: [],
          carTheme: 'car',
          digitalTheme: 'digital',
          carId: 0,
          lookId: 0,
          currentUserId: 0,
          lastTenLook: [],
          showLook: false,
          activeDate: '',
          logo: logo + '?' + +new Date(),
          overviewBg: overviewBg,
          man: man,
          girl: girl,
          heat: heat,
          todayBg: todayBg,
          individuation: individuation,
          like: like,
          color: color,
          car: car,
          popGirl: popGirl,
          popMan: popMan,
          popLike: popLike,
          popCar: popCar,
          popTel: popTel,
          popMessage: popMessage,
          popEye: popEye,
          hub18: hub18,
          hub19: hub19,
          glass_no_privacy: glass_no_privacy,
          glass_has_privacy: glass_has_privacy,
          popCarbg: popCarbg,
          top: 0,
          scatterWidth: '0px',
          windowHeight: document.body.clientHeight,
          windowWidth: document.body.clientWidth,
          carSwiperOption: {
            initialSlide: 0,
            slidesPerView: 1,
            centeredSlides: false,
            autoplayDisableOnInteraction: false,
            grabCursor: true,
            paginationClickable: true,
            nextButton: '.swiper-button-next',
            prevButton: '.swiper-button-prev',
            spaceBetween: 30,
            onSlideChangeEnd(swiper) {
              const id = $this.carList[swiper.activeIndex].car_id;
              let url = window.location.href;
              const carRge = new RegExp($this.carRegExp);
              const carMatch = url.match(carRge);
              if (carMatch) {
                window.location.href = url.replace(carRge, `car=${id}`);
              } else {
                if (url.indexOf("?") > -1) {
                  window.location.href = `${url}&car=${id}`;
                } else {
                  window.location.href = `${url}?car=${id}`;
                }
              }
            }
          },
          dateSwiperOption: {
            initialSlide: 0,
            slidesPerView: 1,
            centeredSlides: false,
            autoplayDisableOnInteraction: false,
            grabCursor: true,
            paginationClickable: true,
            nextButton: '.swiper-button-next',
            prevButton: '.swiper-button-prev',
            spaceBetween: 30,
            onSlideChangeEnd(swiper) {
              const activeDate = $this.originDateList[swiper.activeIndex];
              let url = window.location.href;
              const dayRe = new RegExp($this.dayRegRxp);
              const dayMatch = url.match(dayRe);
              if (dayMatch) {
                window.location.href = url.replace(dayRe, `day=${activeDate}`);
              } else {
                if (url.indexOf("?") > -1) {
                  window.location.href = `${url}&day=${activeDate}`;
                } else {
                  window.location.href = `${url}?day=${activeDate}`;
                }
              }
            }
          },
          swiperOption: {
            initialSlide: 0,
            slidesPerView: 0,
            direction: 'vertical',
            centeredSlides: false,
            autoplayDisableOnInteraction: false,
            autoplay: 2500,
            grabCursor: true,
            paginationClickable: true,
            spaceBetween: 0,
            loop: true,
          },
          currentUsers: [

          ],
          swiperGroup: [],
          activeBg: 'activeBg',
          showUserOverview: false,
          UserOverviewHeight: 0,
          summary: {
            id: 0,
            username: null,
            phone: null,
            lookcar: null,
            intention: null,
            sex: null,
            age: null,
            focus: [],
            color: null,
            wheel: null,
            glass: null,
            voice_time: 0,
            communicate_time: 0,
            budget: null,
            online: false,
            features: [],
          },
          carList: [],
          dateList: [],
          originDateList: [],
          individuationList: [],
          averageLookTime: null,
          mrate: null,
          wrate: null,
          totalUsers: null,
          currentOnlineUsers: null,
          currentLookBody: null,
          currentLookColor: {
            name: null,
            value: null,
          },
          currentLookGlass: null,
          currentLookWheelhub: null,
          carColorList: [],
          activeUser: [],
          activeUid: 0,
          purchaseList: [],
          carfoucus: [],
          carRegExp: 'car=(\\d+)',
          dayRegRxp: 'day=(\\w+-\\w+-\\w+)',
          basicUrl: '/dealer/factory/overview/',
          focusUrl: '/dealer/users/experience/report/',
          onlineUrl: '/dealer/screen/online/users',
          userDetailUrl: '/dealer/users/online/',
          wsBaseUrl: 'wss://dms.autoforce.net/ws',
          focusWsUrl: '/dealer/screen/dynamic/focus',
          focusWs: null,
          onlineCount: 0,
          offlineCount: 0,
          currentSlideDateIndex: 0,
          currentSlideCarIndex: 0,
          userDetailHeaderBg: '#adb5cd',
          userIconColors: ['#6ce7c3', '#e9d547', '#6ce7c3', '#dc4c5c', '#37b8fd', '#e9d547'],
        }
      },
      mounted() {
        const $this = this;
        this.getUrlHash();
        this.fecthOnlineDatas();
        this.scatterWidth = this.$refs.scatter.offsetHeight * 0.98 * 487 / 973  + 30 + 'px';
        this.windowHeight = document.body.clientHeight;
        this.windowWidth = document.body.clientWidth;
        window.addEventListener('resize', this.handleResize);
        window.addEventListener('click', this.handleShowOrHideDetail);
        this.handleNotHideDetail();
        this.createTextAutoSlide();
      },
      computed: {
        communicate() {
          return this.summary.communicate_time > 0 ? `${this.secondsToMinutes(this.summary.communicate_time)}min` : '0min';
        },
        voiceTime() {
          return `${this.summary.voice_time}s`;
        },
        perVw() {
          return this.windowWidth / 100;
        },
        perVh() {
          return this.windowHeight / 100;
        },
        carSwiper() {
          return this.$refs.carSwiper.swiper;
        },
        dateSwiper() {
          return this.$refs.dateSwiper.swiper;
        },
        scrollText() {
          return this.scrollTextList.join("\xa0\xa0\xa0\xa0\xa0");
        },
        userAge() {
          return this.summary.age ? `${this.summary.age}岁` : '';
        },
        userDetailIndividuaName() {
          return this.summary.online ? '正在关注的个性化配件' : '重点关注的个性化配件';
        },
        userDetailCarBodyName() {
          return this.summary.online ? '正在关注的车辆局部' : '重点关注的个性化配件';
        },
        offlineHub() {
          return this.summary.wheel == "18寸轮毂" ? hub18 : hub19;
        },
        offlineGlass() {
          return this.summary.wheel == "黑色隐私玻璃" ? glass_has_privacy : glass_no_privacy;
        },
        onlineHub() {
          return this.currentLookWheelhub == "18寸轮毂" ? hub18 : hub19;
        },
        onlineGlass() {
          return this.currentLookGlass == "黑色隐私玻璃" ? glass_has_privacy : glass_no_privacy;
        },
        currentOnlineBodyLook() {
          return this.currentLookBody ? this.currentLookBody : "暂未关注";
        },
        noCurrentLookIndividuation() {
          console.log("!(this.currentLookWheelhub || this.currentLookGlass || this.currentLookColor.name): ", !(this.currentLookWheelhub || this.currentLookGlass || this.currentLookColor.name))
          return !(this.currentLookWheelhub || this.currentLookGlass || this.currentLookColor.name);
        },
      },
      watch: {
        windowHeight(val) {
          this.windowHeight = val;
        },
        windowWidth(val) {
          this.windowWidth = val;
        },
        currentUsers(val) {
          this.swiperGroup = val;
        },
      },
      beforeDestroy() {
        window.removeEventListener('resize', this.handleResize);
        window.removeEventListener('click', this.handleShowOrHideDetail);
        if (this.focusWs) {
          this.focusWs.close();
        }
      },
      methods: {
        createTextAutoSlide() {
          $('.current-view').liMarquee({
            loop: -1,
            circular: true,
          });
        },
        getUrlHash() {
          const url = window.location.href;
          const carMatch = url.match(new RegExp(this.carRegExp));
          const dayMatch = url.match(new RegExp(this.dayRegRxp));
          this.carId = carMatch ? parseInt(carMatch[1]) : this.getCarId();
          if (this.ifIsToday(this.activeDate)) {
            this.createfocusWs(this.carId);
          }
          this.activeDate = dayMatch ? dayMatch[1] : this.parseSelectDay(new Date());
          this.fetchBasicDatas();
          setTimeout(() => {
            this.saveCarId(this.carId);
          }, 500)
        },
        getCarId() {
          if (!window.localStorage) {
            return 29524;
          }
          const carId = window.localStorage.getItem('carid');
          return carId ? carId : 29524;
        },
        saveCarId(carid) {
          if (!window.localStorage) {
            return;
          }
          window.localStorage.setItem('carid', carid);
        },
        createfocusWs(carid) {
          if (!carid) {
            return;
          }

          this.focusWs = new WebSocket(`${this.wsBaseUrl}${this.focusWsUrl}?car_id=${carid}`);

          this.focusWs.onmessage = (msg) => {
            console.log("focusWsMsg: ", msg.data);
            if (this.ifIsToday(this.activeDate)) {
              if (msg.data) {
                const data = JSON.parse(msg.data);
                if (data.screen_all_look) {
                  this.handleScrollText(data.screen_all_look);
                } else if (data.screen_user_look) {
                  this.parseFocusAndColor(data.screen_user_look);
                } else if (data.screen_part_look) {
                  this.handleColorPartLook(data.screen_part_look);
                } else if (data.on_off_line) {
                  this.handleOnlineList(data.on_off_line);
                }
              }
            }
          }

          this.focusWs.onopen = (msg) => {
            console.log("focusWsopen: ", msg);
          }

          this.focusWs.onclose = (msg) => {
            console.log("focusWsclose: ", msg);
          }
        },
        handleScrollText(data) {
          if (this.scrollTextList.length > 10) {
            this.scrollTextList.splice(0, 1);
          }
          this.scrollTextList.push(data);
        },
        handleColorPartLook(data) {
          this.lastTenLook = [];
          this.lastTenLook = [data];
        },
        parseFocusAndColor(detail) {
          if (_.has(detail, 'body')) {
            this.currentLookBody = _.get(detail, 'body');
          }
          if (_.has(detail, 'color')) {
            this.currentLookColor.name = _.get(detail, 'color').name;
            this.currentLookColor.value = _.get(detail, 'color').value;
          }
          if (_.has(detail, 'glass')) {
            this.currentLookGlass = _.get(detail, 'glass');
          }
          if (_.has(detail, 'wheelhub')) {
            this.currentLookWheelhub = _.get(detail, 'wheelhub');
          }
        },
        setCarSwiperActive() {
          const carActiveIndex = _.findIndex(this.carList, (o) => {
            return o.car_id ===  this.carId;
          })
          if (this.currentSlideCarIndex === carActiveIndex) {
            return;
          }
          this.currentSlideCarIndex = carActiveIndex;
          this.carSwiper.slideTo(carActiveIndex, 700, false);
        },
        setDateSwiperActive() {
          const dateActiveIndex = _.findIndex(this.dateList, (date) => {
            return date ===  this.parseShowDate(this.activeDate);
          })
          if (this.currentSlideDateIndex === dateActiveIndex) {
            return;
          }
          this.currentSlideDateIndex = dateActiveIndex;
          this.dateSwiper.slideTo(dateActiveIndex, 700, false);
        },
        launchFullscreen(element) {
          if(element.requestFullscreen) {
            element.requestFullscreen();
          } else if(element.mozRequestFullScreen) {
            element.mozRequestFullScreen();
          } else if(element.msRequestFullscreen){
            element.msRequestFullscreen();
          } else if(element.webkitRequestFullscreen) {
            element.webkitRequestFullScreen();
          }
        },
        fullScreen() {
          this.launchFullscreen(document.documentElement);
        },
        handleResize() {
          this.windowHeight = document.body.clientHeight;
          this.windowWidth = document.body.clientWidth;
          this.scatterWidth = (this.$refs.scatter.offsetHeight * 0.98 * 408 / 895 + 25) + 'px';
          // console.log("scatterWidth", this.scatterWidth);
        },
        secondsToMinutes(seconds) {
          return Math.round(seconds / 60);
        },
        handleShowOverviewInterval() {
          setTimeout(this.getRandomAutoShowIndex, 1000);
        },
        getRandomAutoShowIndex() {
          const len = this.swiperGroup.length;
          if (len > 0) {
            const index = Math.floor(Math.random() * len);
            // const showOne = this.swiperGroup.splice(index, 1);
            const showOne = this.swiperGroup[index];
            this.handleAutoShow(showOne);
          } else {
            this.handleShowOverviewInterval();
          }
        },
        handleAutoShow(uid) {
          const index = _.findIndex(this.$refs.mySwiper.$children, (obj) => {
            return obj.$el.dataset.uid == uid;
          })
          const target = this.$refs.mySwiper.$children[index].$el;
          const {top, height} = target.getBoundingClientRect();
          this.handleShowOverview(top + height / 2, target);
        },
        fetchBasicDatas() {
          const $this = this;
          axios.get(`${this.basicUrl}${this.carId}?day=${this.activeDate}`)
          .then(response => {
            // console.log("response: ", response);
            // console.log("activeDate: ", this.activeDate);
            if (response.status === 200) {
              $this.assignBasicDatas(response.data);
            }
            setTimeout($this.fetchBasicDatas, 5000);
          })
          .catch(error => {
            console.log(error);
            setTimeout($this.fetchBasicDatas, 5000);
          });
        },
        fecthFocusDatas() {
          const $this = this;
          axios.get(`${this.focusUrl}${this.lookId}?t=${+new Date()}`)
          .then(response => {
            $this.assignFocusDatas(response.data)
            setTimeout($this.fecthFocusDatas, 5000);
          })
          .catch(error => {
            console.log(error);
            setTimeout($this.fecthFocusDatas, 5000);
          });
        },
        fecthOnlineDatas() {
          const $this = this;
          axios.get(`${this.onlineUrl}?car_id=${this.carId}&day=${this.activeDate}`)
          .then(response => {
            $this.assignOnlineDatas(response.data.data)
          })
          .catch(error => {
            console.log(error);
          });
        },
        assignBasicDatas(parsed) {
          // console.log("parsed: ", parsed);
          if (parsed) {
            this.averageLookTime = parsed.avg_look_time;
            this.wrate = parsed.wrate;
            this.mrate = parsed.mrate;
            this.handleCurrentOnlineUsers(parsed.vr_num);
            this.totalUsers = parsed.total_users_num;
            this.carColorList = parsed.look_colors;
            this.activeUser = parsed.active_user_num.date;
            this.purchaseList = parsed.car_intention;
            this.carfoucus = _.sortBy(parsed.carfoucus, [function(data) {
              return -data[0];
            }]);
            this.carList = parsed.dealer_car;
            this.setCarSwiperActive();
            this.assignIndividuationList(parsed.selfhood_select);
            this.assignDate(parsed.date);
          }
        },
        handleCurrentOnlineUsers(users) {
          if (this.ifIsToday(this.activeDate)) {
            this.currentOnlineUsers = users;
          } else {
            this.currentOnlineUsers = 0;
          }
        },
        assignIndividuationList(inObj) {
          let res = [];
          _.forEach(inObj, (o, key) => {
            if (key.includes('glass_0')) {
              res.push({
                look_times: o.look_times,
                part_name: o.part_name,
                time: o.time,
                src: glass_no_privacy,
              })
            } else if (key.includes('glass_1')) {
              res.push({
                look_times: o.look_times,
                part_name: o.part_name,
                time: o.time,
                src: glass_has_privacy,
              })
            } else if (key.includes('wheelhub_A')) {
              res.push({
                look_times: o.look_times,
                part_name: o.part_name,
                time: o.time,
                src: hub18,
              })
            } else if (key.includes('wheelhub_B')) {
              res.push({
                look_times: o.look_times,
                part_name: o.part_name,
                time: o.time,
                src: hub19,
              })
            }
          })
          let orderRes = _.orderBy(res, ['part_name']);
          orderRes.splice(2, 0, orderRes.splice(1, 1)[0]);
          this.individuationList = orderRes;
        },
        assignDate(date) {
          this.originDateList = date;
          this.dateList = _.map(date, (o) => {
            return this.parseShowDate(o);
          });
          this.setDateSwiperActive();
        },
        assignFocusDatas(parsed) {
          if (parsed && parsed.data) {
            if (parsed.data.latest) {
              this.randerCurrentLook(parsed.data.latest);
            }
            this.lastTenLook = parsed.data.look_list;
            if (parsed.data.user) {
              this.ifNewOnlineUser(parsed.data);
              this.ifHasUsers(parsed.data.user);
            }
          }
        },
        assignOnlineDatas(data) {
          const $this = this;
          this.onlineCount = data.online && data.online.num;
          this.offlineCount = data.offline && data.offline.num;
          this.currentUsers = [];
          console.log("first online: ", data);
          if (data.online) {
            _.forEach(data.online.users, (online) => {
              this.currentUsers.push(this.formatOnlineUser(online));
            })
          }
          if (data.offline) {
            _.forEach(data.offline.users, (offline) => {
              this.currentUsers.push(this.formatOfflineUser(offline));
            })
          }
        },
        ifIsToday(activeDate) {
          const now = +new Date(this.parseSelectDay(new Date()));
          const active = +new Date(activeDate);
          if (active < now) {
            return false;
          }
          return true;
        },
        getRandomIconBg() {
          return this.userIconColors[Math.floor(Math.random() * this.userIconColors.length)];
        },
        handleOnlineList(online) {
          console.log("online change: ", online);
          if (!_.keys(online).length) {
            return;
          }
          const exist = _.findIndex(this.currentUsers, (o) => {
            return o.id === online.id;
          });
          if (exist > -1) {
            const oldOnline = this.currentUsers[exist];
            if (oldOnline.online === online.online) {
              return;
            }
            if (online.online) {
              this.onlineCount++;
              this.offlineCount--;
              this.currentUsers.splice(exist, 1, this.formatOnlineUser(online));
            } else {
              this.offlineCount++;
              this.onlineCount--;
              this.currentUsers.splice(exist, 1, this.formatOfflineUser(online));
            }

          } else {
            if (online.online) {
              this.onlineCount++;
              this.currentUsers.unshift(this.formatOnlineUser(online));
            } else {
              this.offlineCount++;
              this.currentUsers.push(this.formatOfflineUser(online));
            }
          }
        },
        formatOnlineUser(online) {
          return {
            id: online.id,
            name: online.name,
            online: online.online,
            iconBg: this.getRandomIconBg(),
          };
        },
        formatOfflineUser(offline) {
          return {
            id: offline.id,
            name: offline.name,
            online: offline.online,
            iconBg: '#535a6c',
          };
        },
        ifHasUsers(user) {
          const index = _.findIndex(this.currentUsers, function(o) {
            return o.id == user.id;
          })
          if (index > -1) {
            this.currentUsers.splice(index, 1)
          }
          this.currentUsers.push(user);
          if (this.currentUsers.length > 10) {
            this.currentUsers.shift();
          }
        },
        ifNewOnlineUser(parsed) {
          this.lookId = parsed.id;
          if (this.currentUserId != parsed.user.id) {
            this.currentUserId = parsed.user.id;
            setTimeout(() => {
              this.fecthOnlineUsersInfo(parsed.user);
              this.handleAutoShow(parsed.user.id);
            }, 1000);
          }
        },
        randerCurrentLook(look) {
          this.currentLook = look.name + '正在观看' + look.car + look.part;
          this.showLook = true;
          setTimeout(() => {
            this.showLook = false;
          }, 3000)
        },
        toggleLook(cb) {
          this.showLook = true;
          cb();
        },
        fecthOnlineUsersInfo(user) {
          this.summary.id = user.id;
          this.summary.username = user.username;
          this.summary.phone = user.phone;
          this.summary.lookcar = user.lookcar;
          this.summary.intention = user.intention;
          this.summary.sex = user.sex;
          this.summary.focus = user.focus;
          this.summary.color = user.color;
          this.summary.voice_time = isNaN(user.voice_time) ? 0 : user.voice_time;
          this.summary.budget = user.budget;
          this.summary.communicate_time = user.communicate_time;
        },
        fecthUserDetailInfo(user) {
          const $this = this;
          axios.get(`${this.userDetailUrl}${user.id}`)
          .then(response => {
            console.log("online: ", response.data.data)
            const parsed = response.data && response.data.data;
            if (parsed) {
              $this.summary.id = parsed.id;
              $this.summary.username = parsed.username;
              $this.summary.phone = parsed.phone;
              $this.summary.lookcar = parsed.lookcar;
              $this.summary.intention = parsed.intention;
              $this.summary.sex = parsed.sex;
              $this.summary.voice_time = isNaN(parsed.voice_time) ? 0 : parsed.voice_time;
              $this.summary.budget = parsed.budget;
              $this.summary.communicate_time = parsed.communicate_time;
              $this.summary.age = parsed.age;
              $this.summary.online = user.online;
              this.handleSummaryFeature(parsed.feature);
              this.handleSummaryFocusAndColor(user, parsed);
            }
          })
          .catch(error => {
            console.log(error);
          });
        },
        handleSummaryFeature(feature) {
          this.summary.features = [];
          if (feature.has_plate) {
            this.summary.features.push(feature.has_plate);
          }
          if (feature.buy_in) {
            this.summary.features.push(`${feature.buy_in}买车`);
          }
          if (feature.budget) {
            this.summary.features.push(`预算${feature.budget}`);
          }
          if (feature.method) {
            this.summary.features.push(feature.method);
          }
        },
        handleSummaryFocusAndColor(user, parsed) {
          if (!user.online) {
            if (parsed.focus.length >= 5) {
              this.summary.focus = parsed.focus.slice(0, 5);
            } else {
              this.summary.focus = parsed.focus;
            }
            if (parsed.color.length > 0) {
              this.summary.color = parsed.color[0];
            }
            if (parsed.glass.length > 0) {
              this.summary.glass = parsed.glass[0];
            }
            if (parsed.wheel.length > 0) {
              this.summary.wheel = parsed.wheel[0];
            }
            return;
          }
          this.resetCurrentLookDetail();
          this.focusWs.send(JSON.stringify({'uid': user.id}));
        },
        resetCurrentLookDetail() {
          this.currentLookBody = null;
          this.currentLookColor = {
            name: null,
            value: null,
          };
          this.currentLookGlass =  null;
          this.currentLookWheelhub = null;
        },
        parseShowDate(val) {
          const date = new Date(val);
          return (date.getMonth() + 1) + '月' + date.getDate() + '日';
        },
        parseSelectDay(val) {
          const date = new Date(val);
          const year = date.getFullYear();
          const month = date.getMonth() + 1 < 10 ? `0${date.getMonth() + 1}` : date.getMonth() + 1;
          const day = date.getDate() < 10 ? `0${date.getDate()}` : date.getDate();
          return `${year}-${month}-${day}`
        },
        showDetailUserInfo(user, event) {
          this.getUserDetailIconBg(user);
          this.fecthUserDetailInfo(user);
          this.calculateTopHeight(event.target);
          if (user.online && user.id) {
            this.activeUid = user.id;
          } else {
            this.activeUid = 0;
          }
          event.stopPropagation();
        },
        getUserDetailIconBg(user) {
          if (user.online) {
            this.userDetailHeaderBg = user.iconBg;
          } else {
            this.userDetailHeaderBg = '#adb5cd';
          }
        },
        calculateTopHeight(target) {
          const $singleUser = $(target).parents(".single-user");
          const top = $singleUser.offset().top;
          const height = $singleUser.height();
          this.handleShowOverview(top + height / 2, $singleUser);
        },
        handleShowOverview(top, target) {
          $(".arrow").hide();
          const $arrow = target.find(".arrow");
          $arrow.show();
          this.setUserDetailPostion(top, target);
        },
        setUserDetailPostion(top, target) {
          const height = $(".user-detail").height();
          if (top + height / 2 >= this.windowHeight) {
            this.$refs.userDetail.style.top = (this.windowHeight - height) / this.perVw + 'vw';
          } else if (top <= height / 2) {
            this.$refs.userDetail.style.top = '25.9vh';
          } else {
            if ((top - height / 2) <= 25.9 * this.perVh) {
              this.$refs.userDetail.style.top = '25.9vh';
            } else {
              this.$refs.userDetail.style.top = (top - height / 2) / this.perVw + 'vw';
            }
          }
          this.showUserOverview = true;
        },
        handleHideOverview(uid) {
          $(".arrow").hide();
          this.showUserOverview = false;
          if (uid) {
            this.focusWs.send(JSON.stringify({'close_uid': uid}));
          }
        },
        handleShowOrHideDetail() {
          if (this.showUserOverview) {
            this.handleHideOverview(this.activeUid);
          }
        },
        handleNotHideDetail() {
          $(".user-detail").on('click', (e) => {
            e.stopPropagation();
          });
        },
      },
    }
</script>

<style lang="scss" scoped>
@mixin nopaddingmargin {
  margin: 0;
  padding: 0;
}
@mixin wh($width, $height) {
  width: #{$width};
  height: #{$height};
}

@mixin bg($color) {
  background: #{$color};
}

@mixin padding($top, $right, $bottom, $left) {
  padding-top: #{$top};
  padding-right: #{$right};
  padding-bottom: #{$bottom};
  padding-left: #{$left};
}

@mixin margin($top, $right, $bottom, $left) {
  margin-top: #{$top};
  margin-right: #{$right};
  margin-bottom: #{$bottom};
  margin-left: #{$left};
}

@mixin borderradius($top, $right, $bottom, $left) {
  border-top-left-radius: $top;
  border-top-right-radius: $right;
  border-bottom-left-radius: $bottom;
  border-bottom-right-radius: $left;
}

$mainBg: '#353b4c';
$mainShadows: 0 1px 0 0 rgba(0, 0, 0, 0.3);

@mixin flex($direction, $justify, $align) {
  display: flex;
  flex-direction: #{$direction};
  justify-content: #{$justify};
  align-items: #{$align};
}
.screen-container {
  @include wh(100vw, 100vh);
  box-sizing: border-box;
  position: relative;
  color: #fff;
  .screen-header {
    @include wh(100vw, 8.6vh);
    @include bg($mainBg);
    @include flex('row', 'flex-start', 'center');
    box-shadow: $mainShadows;

    .logo {
      @include flex('row', 'center', 'center');
      @include wh(13.9vw, 8.6vh);
      @include bg('#b52f25');
      .logo-img {
        @include wh(auto, 6vh);
        max-width: 90%;
      }
    }
    .title {
      @include wh(86.1vw, 8.6vh);
      @include flex('row', 'flex-start', 'center');

      .company-message {
        @include wh(41vw, 8.6vh);
        @include flex('row', 'flex-start', 'center');
        padding-left: 1.1vw;
        // font-size: 0.8vw;
        font-size: calc(12px + 0.25vw);

        .company-sub-title {
          margin-left: 0.6vw;
          color: #7d7f90;
          // font-size: 0.6vw;
          font-size: calc(12px + 0.15vw);
        }
      }
      .scroll-message {
        @include wh(45.1vw, 8.6vh);
        @include flex('row', 'flex-start', 'center');

        .current-view {
          @include wh(17vw, 4.3vh);
          height: 4.3vh !important;
          line-height: 4.3vh !important;
          margin-right: 0.77vw;
          background: #272e3c;
          // font-size: 0.7vw;
          font-size: calc(12px + 0.15vw);

          .scroll-text {
            margin-right: 10px;
            display: inline-block;
          }
        }

        .current-lookcar {
          @include wh(14.4vw, 4.3vh);
          margin-right: 0.77vw;
        }
        .current-day {
          @include wh(10.4vw, 4.3vh);
        }
      }
    }
  }

  .content {
    position: relative;
    @include wh(100vw, 91.4vh);
    @include bg('#252735');
    @include flex('row', 'flex-start', 'flex-start');

    &-left {
      position: relative;
      @include flex('row', 'flex-start', 'flex-start');
      @include wh(93.7vw, 91.4vh);
      @include padding(3.5vh, 2vw, 3.4vh, 1.6vw);

      .overview {
        @include flex('column', 'flex-start', 'center');

        &-title {
          @include wh(12.3vw, 6.2vh);
          @include flex('row', 'flex-start', 'center');
          @include bg('#353b4d');
          margin-bottom: 1px;
          box-shadow: $mainShadows;

          &-bg {
            @include wh(3vw, 6.2vh);
            @include flex('row', 'center', 'center');
          }
          &-name {
            @include flex('column', 'center', 'flex-start');
          }
        }
        &-four-part {
          @include flex('column', 'center', 'center');
          @include wh(12.3vw, 18vh);
          @include bg('#2e3342');
          border-radius: 3px;
          border-top-left-radius: 0;
          border-top-right-radius: 0;
          box-shadow: $mainShadows;
          & > p {
            @include nopaddingmargin;
            margin-bottom: 2vh;
          }
          & > p:last-child {
            margin-bottom: 0;
          }
        }

      }

      .core-map {
        @include flex('column', 'flex-start', 'flex-start');
        @include bg('#2e3342');
        @include borderradius(0, 0, 3px, 3px);
        box-shadow: 0 0 40px 0 rgba(0, 0, 0, 0.3);
      }
      .core-heat {
        padding: 0.8vw;
        @include flex('row', 'flex-start', 'center');

        &-half {
          @include flex('row', 'center', 'center');
          @include wh(50%, 100%);
          position: relative;
        }
        &-normal {
          background: #272e3c;
        }
        .car {
          @include wh(auto, 98%);
        }
        .focus-scatter {
          position: absolute;
          height: 98%;
          left: 50%;
          top: 50%;
          transform: translate(-50%, -50%);
        }
      }
      .core-individuation {
        @include flex('row', 'space-between', 'center');
        flex-wrap: wrap;
        align-content: space-between;
        padding: 0.8vw;


        .individuation-item {
          background: #272e3c;
          width: 49%;
          height: 48%;
          @include flex('row', 'flex-start', 'center');

          .individuation-overview-title {
            // font-size: 0.7vw;
            font-size: calc(12px + 0.15vw);
          }
          .individuation-num {

            & > span:first-child {
              width: auto;
              min-width: 1.5vw;
              display: inline-block;
              text-align: center;
              // font-size: 0.8vw;
              font-size: calc(12px + 0.25vw);
            }
            & > span:last-child {
              // font-size: 0.45vw;
              font-size: 12px;
              color: #7d7f90;
            }
          }
          .individuation-overview {
            @include flex('column', 'space-around', 'flex-start');
            @include wh(50%, 85%);

            p {
              @include wh(100%, '');
              @include nopaddingmargin;
              overflow: hidden;
              text-overflow: ellipsis;
              white-space: nowrap;
            }
          }
          .individuation-img {
            @include wh(40%, 100%);
            @include flex('column', 'center', 'center');
            margin: 0 5% 0 5%;

            & > img {
              @include wh(100%, auto);
            }
          }
        }
      }
      .core-color {
        @include padding(2vh, 1vw, 2vh, 1vw);
        @include flex('column', 'space-between', 'flex-start');

        &-item {
          @include wh(100%, '');
          @include flex('row', 'flex-start', 'center');
          border-bottom: 1px solid #353b4c;

          &-left {
            @include wh(60%, '');
            @include flex('row', 'flex-start', 'center');

            .color-bg {
              width: 1vw;
              height: 1vw;
              position: relative;
              border: 2px solid #fff;
              margin-right: 0.5vw;
              box-sizing: content-box;

              &-arrow {
                position: absolute;
                left: 0;
                top: 0;
                opacity: 0.2;
                border-top: 1vw solid #fff;
                border-right: 1vw solid transparent;
              }
            }
            .color-name {
              // font-size: 0.7vw;
              font-size: calc(12px + 0.15vw);
            }

          }
          &-right {
            @include wh(40%, '');
            @include flex('row', 'flex-end', 'center');

            .color-rate-num {
              // font-size: 0.8vw;
              font-size: calc(12px + 0.25vw);
              margin-right: 0.5vw;
            }
            .color-chart {
              @include wh(2vw, 2vw);
            }
          }
        }
      }
      .color-purchase-wrapper {
        @include wh(36.8vw, 44.6vh);
        @include flex('row', 'flex-start', 'flex-start');
      }
      .today-look {
        @include wh(36.8vw, 38.4vh);
      }

      .purchase-color-chart-wrapper {
        @include flex('column', 'flex-start', 'flex-start');
        background: #2e3342;
        @include wh(38.8vw, 38.4vh);
      }

      .individuation {
        @include wh(38.8vw, 44.6vh);
      }
      .purchase,
      .color-preference {
        @include wh(18.8vw, 44.6vh);
      }
    }

    &-right {
      @include wh(6.3vw, 91.4vh);
      @include bg($mainBg);
      box-shadow: $mainShadows;

      .online-offline {
        @include wh(6.3vw, 17.3vh);
        border-top: 1px solid #191c24;
        border-bottom: 1px solid #191c24;
        padding: 1vw 0;
        @include flex('column', 'space-around', 'center');

        & > div {
          max-width: 80%;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }

        .onoff-title {
          color: #7d7f90;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          // font-size: 0.5vw;
          font-size: calc(12px + 0.01vw);
        }
        .onoff-top {
          @include flex('row', 'flex-start', 'baseline');

          & > span:first-child {
            @include wh(0.5vw, 0.5vw);
            border-radius: 50%;
            margin: 0 1vw 0.5vw 0;

            &.online-status {
              background-color: #3bcc68;
            }
            &.offline-status {
              background-color: #545a6c;
            }
          }
          & > span:last-child {
            // font-size: 0.8vw;
            font-size: calc(12px + 0.25vw);
          }
        }
      }
      .current-online {
        @include wh(6.3vw, 74.1vh);
      }
    }
    .current-user {
      @include wh(6.8vw, 74.1vh);
      position: fixed;
      right: 0;
      bottom: 0;
      z-index: 9999;
      @include flex('column', 'flex-start', 'center');
    }
    .current-user .single-user {
      position: relative;
      @include flex('column', 'center', 'center');
      @include wh(6.8vw, 6.8vw);
      margin-left: 0.25vw;
      margin-bottom: 0.9%;
    }
    // .current-user .single-user.active {
    //   background: #f03635;
    // }
    .current-user .single-user .single-user-name-box {
      @include flex('column', 'center', 'center');
      @include wh(4.3vw, 4.3vw);
      border-radius: 50%;
    }
    .current-user .single-user .single-user-name-box .single-user-name {
      width: auto;
      max-width: 90%;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      // font-size: 0.8vw;
      font-size: calc(12px + 0.25vw);
    }
    .swiper-slide {
      width: 100%;
    }
  }

  .core-mb {
    @include margin('', '', 2.4vh, '');
  }

  .core-mr {
    @include margin('', 1.1vw, '', '');
  }

  .scroll-style {
    color: #ff4b4c;
    border-radius: 30px;
    min-height: 20px;
    @include flex('column', 'center', 'center');
  }

  .scroll-swiper {
    border: 1px solid #ff4b4c;
    border: 1px solid #ff4b4c;
    text-align: center;
    // font-size: 0.7vw;
    font-size: calc(12px + 0.15vw);

    & > div {
      width: 100%;
    }
    .slide-inner-text {
      display: inline-block;
      max-width: 70%;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .swiper-slide {
      line-height: 1em;
    }
  }

  .swiper-button-red {
    background-image: none;
  }

  .swiper-button-prev,
  .swiper-button-next {
    height: 4.3vh !important;
    line-height: 4.3vh !important;
    font-size: 1vw;
    margin-top: -2.15vh;
  }

  .swiper-button-prev.swiper-button-red::after {
    content: '<';
  }

  .swiper-button-next.swiper-button-red::after {
    content: '>';
  }

  .overview-img {
    @include wh(1.2vw, auto);
  }

  .info-number {
    // font-size: 2vw;
    font-size: calc(12px + 1.5vw);

    &-sex {
      @include flex('row', 'space-between', 'center');
      @include wh(100%, auto);
    }
    .sex {
      @include wh(45%, auto);
      // font-size: 1vw;
      font-size: calc(12px + 0.3vw);
    }
    .sex-boy {
      @include flex('row', 'flex-end', 'center');
    }
    .sex-girl {
      @include flex('row', 'flex-start', 'center');
    }
    .sex-icon {
      @include wh(auto, 3vh);
      margin-right: 0.5vw;
    }
  }

  .divide-line {
    @include wh(3vw, 2px);

    &-red {
      background-color: #ff4b4c;
    }
    &-yellow {
      background-color: #fec62b;
    }
    &-purple {
      background-color: #a639d2;
    }
    &-green {
      background-color: #3bcc68;
    }
  }

  .info-title {
    color: #7d7f90;
    // font-size: 0.7vw;
    font-size: calc(12px + 0.15vw);
  }

  .core-header {
    @include flex('row', 'flex-start', 'center');
    @include wh(100%, 6.2vh);
    @include bg('#353b4c');
    @include borderradius(3px, 3px, 0, 0);
    box-shadow: $mainShadows;

    &-icon {
      @include wh(2.8vw, '');
      @include flex('row', 'flex-end', 'center');
      margin-right: 0.5vw;
      img {
        @include wh(40%, auto);
      }
    }
    &-title {
      @include flex('column', 'center', 'flex-start');
    }
  }

  .core-content-top {
    @include wh(100%, 37.27vh);
  }

  .core-content-btm {
    @include wh(100%, 30.17vh);
  }

  .color-purchase-content {
    @include wh(100%, 36.37vh);
  }

  .main-title {
    @include nopaddingmargin;
    // font-size: 0.75vw;
    font-size: calc(12px + 0.2vw);
  }

  .sub-title {
    @include nopaddingmargin;
    color: #7d7f90;
    // font-size: 0.5vw;
    font-size: calc(12px + 0.01vw);
  }

  .arrow {
    position: absolute;
    width: 0;
    height: 0;
    border-style: solid;
    border-color: transparent;
  }

  .left-arrow {
    top: 50%;
    margin-top: -0.5vw;
    left: -0.75vw;
    border-width: 0.5vw 0.5vw 0.5vw 0.5vw;
    border-right-color: #353b4d;
    display: none;
  }

  .user-detail {
    position: fixed;
    right: 6.3vw;
    top: 25.9vh;
    z-index: 9998;
    @include flex('column', 'flex-start', 'flex-start');
    @include wh(21vw, 36.6vw);
    padding: 0 1vw;
    background-color: #f7f7f7;
    box-shadow: 0 0 40px 0 rgba(0, 0, 0, 0.3);
    border-radius: 3px;

    &-line {
      border-bottom: 1px solid #ddd;
    }
    &-header {
      @include wh(100%, 22.7%);
      @include flex('column', 'space-around', 'center');
      color: #333;
      padding: 2.15vw 0 0.8vw;

      & > div {
        @include flex('row', 'center', 'center');
      }

      .user-icon {
        @include wh(4.3vw, 4.3vw);
        @include flex('column', 'center', 'center');
        position: absolute;
        top: 0;
        border-radius: 50%;
        margin-top: -2.15vw;

        span {
          display: inline-block;
          max-width: 90%;
          width: auto;
          color: #fff;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          // font-size: 0.8vw;
          font-size: calc(12px + 0.25vw);
        }
      }
      .user-username {
        color: #22222e;
        // font-size: 0.75vw;
        font-size: calc(12px + 0.2vw);
      }
      .user-phone {
        @include flex('row', 'center', 'center');
        color: #9294a3;
        // font-size: 0.65vw;
        font-size: calc(12px + 0.01vw);

        .user-sex {
          @include wh(1vw, auto);
        }
        .user-phone-gap {
          margin-right: 0.5vw;
        }
      }

      .user-tags {
        @include flex('row', 'center', 'center');
        @include wh(100%, auto);

        span {
          padding: 0.01vw 0.5vw;
          background-color: #adb5ce;
          border-radius: 1px;
          color: #fff;
          margin-right: 0.3vw;
          width: auto;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          // font-size: 0.5vw;
          font-size: calc(12px + 0.01vw);
        }
      }
    }
    &-phone {
      @include wh(100%, 24.5%);
      @include flex('row', 'center', 'center');
      padding: 1vw;

      .phone-left {
        @include wh(50%, 100%);
      }
      .phone-right {
        @include wh(50%, 100%);
        padding-left: 1vw;
      }
      .phone-wrapper {
        @include flex('column', 'space-around', 'center');
      }
      .phone-wrapper-inner {

        .phone-wrapper-box {
          @include flex('row', 'flex-start', 'center');
          // margin-bottom: 0.2vw;
          width: 100%;

          span {
            color: #999;
            // font-size: 0.5vw;
            font-size: calc(12px + 0.01vw);
          }
          img,
          .phone-wrapper-placeholder {
            @include wh(1vw, auto);
            margin-right: 0.5vw;
          }
          span.phone-count {
            color: #333;
            // text-overflow: ellipsis;
            // white-space: nowrap;
            // font-size: 0.65vw;
            border-radius: 2px;
            font-size: calc(12px + 0.15vw);

            &.focus-car {
              font-size: 12px;
            }
            &.purchase-count {
              background-color: #ff5f60;
              color: #fff;
              padding: 0 10px;
            }
          }
        }
      }
      .phone-wrapper-inner-top {
        @include flex('column', 'flex-start', 'flex-start');
        @include wh(100%, 60%);
      }
      // .focus-car-outer {
      //   @include flex('column', 'space-between', 'flex-start');
      // }
      .phone-wrapper-inner-btm {
        @include flex('column', 'flex-end', 'flex-start');
        @include wh(100%, 40%);
      }
    }
    &-individuation {
      @include wh(100%, 27.8%);
      @include flex('column', 'space-between', 'flex-start');
      padding: 1vw 0 0.5vw;

      &-btm {
        @include flex('row', 'flex-start', 'center');
        @include wh(100%, 100%);

        .focus-individuation-item {
          @include flex('column', 'space-around', 'center');
          @include wh(32%, 80%);
          color: #7d7f90;
          margin-right: 0.3vw;
          border: 1px solid #37b8fd;
          position: relative;
          padding: 0.3vw 0;
          font-size: calc(12px + 0.01vw);

          .look-color {
            position: relative;
            @include wh(2vw, 2vw);

            &-arrow {
              position: absolute;
              left: 0;
              top: 0;
              opacity: 0.2;
              border-top: 2vw solid #fff;
              border-right: 2vw solid transparent;
            }
          }

          .color-wrapper {
            height: 60%;
            @include flex('column', 'center', 'center');
          }
          img {
            @include wh(auto, 60%);
          }
          span {
            display: inline-block;
            width: 100%;
            // min-height: 1.4vw;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            text-align: center;
          }
        }
      }
    }
    &-fouce {
      @include wh(100%, 25%);
      padding: 1vw 0 0.5vw;
      @include flex('column', 'flex-start', 'flex-start');

      .focus-part-title {
        margin-bottom: 5%;
      }
      .focus-part {
        @include flex('row', 'flex-start', 'baseline');
        flex-wrap: wrap;
        @include wh(100%, auto);

        .focus-now-box {
          position: relative;
          @include flex('row', 'space-between', 'center');
          border-radius: 5px;
          background-color: #37b9fd;
          @include wh(100%, 5vw);

          .focus-now-icon {
            @include wh(auto, 85%);
          }
          .focus-now-name-wrapper {
            width: auto;
            position: absolute;
            right: 1vw;
            bottom: 0;
            height: 100%;
            min-width: 6vw;
            @include flex('column', 'center', 'flex-end');

            .focus-now-name {
              // font-size: 1vw;
              font-size: calc(12px + 0.25vw);
            }
            .focus-now-title {
              position: absolute;
              bottom: 0.3vw;
              // font-size: 0.38vw;
              font-size: 12px;
            }
          }
        }
        & > span {
            padding: 0.06vw;
            background-color: #37b9fd;
            border-radius: 1px;
            color: #fff;
            width: 23%;
            margin-right: 2%;
            text-align: center;
            margin-bottom: 2%;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            line-height: 1.5em;
            font-size: calc(12px + 0.01vw);
          }
      }
    }
    .focus-title {
      @include flex('row', 'flex-start', 'center');

      img {
        @include wh(1vw, auto);
        margin-right: 0.5vw;
      }
      &-name {
        color: #333333;
        // font-size: 0.5vw;
        font-size: calc(12px + 0.01vw);
      }
    }

    .arrow-left-top {
      position: absolute;
      left: 0;
      top: 0;
      border-top: 0.5vw solid #37b8fd;
      border-right: 0.5vw solid transparent;
    }

    .arrow-right-btm {
      position: absolute;
      right: 0;
      bottom: 0;
      border-bottom: 0.5vw solid #37b8fd;
      border-left: 0.5vw solid transparent;
    }
  }

  .str_move > span {
    display: inline-block;
  }

  .odometer.odometer-auto-theme, .odometer.odometer-theme-default {
    font-family: "Helvetica Neue", "Helvetica Neue", Helvetica, Arial, "Hiragino Sans GB", "Microsoft Yahei", sans-serif;
    font-weight: 200;
  }
  .people-unit {
    font-size: calc(12px + 0.01vw);
  }

  .current-look-individuation {
    color: #999;
    @include wh(100%, auto);
    @include flex('row', 'center', 'center');
  }

  .current-has-look-individuation {
    @include flex('row', 'flex-start', 'center');
    @include wh(100%, 100%);
  }

  @media (min-width: 1700px) {
    .company-message {
      font-size: 0.8vw !important;
    }

    .company-sub-title {
      font-size: 0.6vw !important;
    }

    .current-view {
      font-size: 0.7vw !important;
    }

    .individuation-overview-title {
      font-size: 0.7vw !important;
    }

    .individuation-num {

      & > span:first-child {
        font-size: 0.8vw !important;
      }
      & > span:last-child {
        font-size: 0.45vw !important;
      }
    }

    .color-name {
      font-size: 0.7vw !important;
    }

    .color-rate-num {
      font-size: 0.8vw !important;
    }

    .onoff-title {
      font-size: 0.5vw !important;
    }

    .onoff-top {
      & > span:last-child {
        font-size: 0.8vw !important;
      }
    }

    .current-user .single-user .single-user-name-box .single-user-name {
      font-size: 0.8vw !important;
    }

    .scroll-swiper {
      font-size: 0.7vw !important;
    }

    .info-number {
      font-size: 2vw !important;

      .sex {
        font-size: 1vw !important;
      }
    }

    .info-title {
      font-size: 0.7vw !important;
    }

    .main-title {
      font-size: 0.75vw !important;
    }

    .sub-title {
      font-size: 0.5vw !important;
    }

    .user-icon {

      span {
        font-size: 0.8vw !important;
      }
    }

    .user-username {
      font-size: 0.75vw !important;
    }

    .user-phone {
      font-size: 0.65vw !important;
    }

    .user-tags {
      span {
        font-size: 0.5vw !important;
        padding: 0.1vw 0.5vw !important;
      }
    }

    .phone-wrapper-box {
      span {
        font-size: 0.5vw !important;
      }

      span.phone-count {
        font-size: 0.7vw !important;
      }
    }

    .focus-now-name {
      font-size: 1vw !important;
    }

    .focus-now-title {
      font-size: 0.38vw !important;
    }

    .focus-title {
      &-name {
        font-size: 0.5vw !important;
      }
    }

    .focus-part > span {
      padding: 0.1vw !important;
    }

    .phone-wrapper-inner-top,
    .phone-wrapper-inner-btm {
      height: 50% !important;
    }

    .user-detail-phone {
      padding: 1.5vw !important;
    }

    .screen-container .info-number .sex {
      width: 42% !important;
    }
  }

  @media (max-width: 1200px) {
    .individuation-overview {
      height: 48% !important;
    }
  }

  @media (min-width: 1201px) and (max-width: 1400px) {
    .individuation-overview {
      height: 54% !important;
    }
  }

  @media (min-width: 1401px) and (max-width: 1650px) {
    .individuation-overview {
      height: 62% !important;
    }
  }

  @media (min-width: 1401px) and (max-width: 1700px) {
    .focus-car {
      font-size: calc(12px + 0.15vw) !important;
    }
  }

  @media (min-width: 1651px) and (max-width: 1900px) {
    .individuation-overview {
      height: 75% !important;
    }
  }
  @media (min-width: 1300px) and (max-width: 1579px) {
    .phone-wrapper-box {
      margin-bottom: 0.15vw !important;
    }
  }
  @media (min-width: 1580px) {
    .user-detail-phone {
      padding: 1vw 1vw !important;
    }
    .phone-wrapper-inner-top,
    .phone-wrapper-inner-btm {
      height: 50% !important;
    }
    .phone-wrapper-box {
      margin-bottom: 0.3vw !important;
    }
  }
  @media (min-width: 1350px) and (max-width: 1600px) {
    .user-detail {
      height: 34.5vw !important;
    }
  }
  @media (min-width: 1600px) {
    .user-detail {
      height: 33vw !important;
    }
  }
}
</style>
