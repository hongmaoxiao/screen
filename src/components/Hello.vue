<template>
    <div class="screen-container">
      <div class="screen-header">
        <div class="logo">
          <img :src="logo" class="logo-img" alt="logo">
        </div>
        <div class="title">
          <div class="company-message">
            <h2>车势科技</h2>
            <h3 class="sub-title">汽车行业智能销售专家</h3>
          </div>
          <div class="scroll-message">
            <div class="current-view scroll-style">
              {{scrollText}}
            </div>
            <div class="current-lookcar scroll-style scroll-swiper">
              <swiper :options="dateSwiperOption" ref="carSwiper">
                <swiper-slide
                  v-for="car in carList"
                  key="car.id"
                  :data-uid="car.id"
                >
                  {{ car.name }}
                </swiper-slide>
                <div class="swiper-button-prev swiper-button-red" slot="button-prev"></div>
                <div class="swiper-button-next swiper-button-red" slot="button-next"></div>
              </swiper>
            </div>
            <div class="current-day scroll-style scroll-swiper">
              <swiper :options="dateSwiperOption" ref="dateSwiper">
                <swiper-slide
                  v-for="date in dateList"
                  key="date.value"
                  :data-uid="date.value"
                >
                  {{ date.name }}
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
                  :theme="carTheme"
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
                  :theme="digitalTheme"
                ></i-odometer>
              </p>
              <p class="divide-line divide-line-yellow info-mb"></p>
              <p class="info-title">当前在线人数</p>
            </div>
            <div class="ave-look core-mb overview-four-part">
              <p class="info-number info-mb">
                <i-odometer
                  :value="averageLookTime"
                  :theme="carTheme"
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
                    :theme="carTheme"
                  ></i-odometer>
                  %
                </span>
                <span class="sex-girl sex">
                  <img class="girl sex-icon" :src="girl" alt="girl">
                  <i-odometer
                    :value="wrate"
                    :theme="carTheme"
                  ></i-odometer>
                  %
                </span>
              </p>
              <p class="divide-line divide-line-green info-mb"></p>
              <p class="info-title">男女占比</p>
            </div>
          </div>
          <div class="core-mid core-mr">
            <div class="heat core-map core-mb">
              <div class="core-header">
                <div class="core-header-icon">
                  <img :src="heat" class="heat-img" alt="heat">
                </div>
                <div class="core-header-title">
                  <p class="main-title">客户关注点热力图</p>
                  <p class="sub-title">看车时长热力图</p>
                </div>
              </div>
              <div class="core-content core-content-top"></div>
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
              <div class="core-content core-content-top"></div>
            </div>
            <div class="purchase-color-chart-wrapper">
              <div class="core-map core-mr purchase">
                <div class="core-header">
                  <div class="core-header-icon">
                    <img :src="individuation" alt="purchase">
                  </div>
                  <div class="core-header-title">
                    <p class="main-title">购买意向统计</p>
                    <p class="sub-title">客户购买意向人数</p>
                  </div>
                </div>
                <div class="core-content core-content-btm"></div>
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
                <div class="core-content core-content-btm core-color">
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
                        <carColor :perVw="perVw" :colorIndex="key" :id="'car-preference-chart' + key" :sum="colorSum" :listData="carColorList" height='100%' width='100%' />
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="content-right">
          <div class="online-offline">

          </div>
          <div class="current-online">

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
    import trend from './trend.png';
    import man from './man.svg';
    import girl from './girl.svg';
    import heat from './heat.svg';
    import todayBg from './todayBg.svg';
    import individuation from './individuation.svg';
    import color from './color.svg';

    import car from './car.png';
    import conmen from './conmen.png';

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
        return {
          scrollText: '到南京时，有朋友约去游逛，勾留7了一日；第二日上午便须渡江到浦口，下午上车北去。父亲因为事忙，本已说定不送我，叫旅馆里一个熟识的茶房8陪我同去。他再三嘱咐茶房，甚是仔细。但他终于不放心，怕茶房不妥帖9；颇踌躇10了一会。其实我那年已二十岁，北京已来往过两三次，是没有什么要紧的了。他踌躇了一会，终于决定还是自己送我去。我再三劝他不必去；他只说：“不要紧，他们去不好！',
          carTheme: 'car',
          digitalTheme: 'digital',
          carId: 0,
          lookId: 0,
          currentUserId: 0,
          lastTenLook: [],
          showLook: false,
          logo: logo + '?' + +new Date(),
          overviewBg: overviewBg,
          trend: trend + '?' + +new Date(),
          man: man,
          girl: girl,
          heat: heat,
          todayBg: todayBg,
          individuation: individuation,
          color: color,

          car: car + '?' + +new Date(),
          conmen: conmen + '?' + +new Date(),
          top: 0,
          today: this.getDate(+new Date()),
          scatterWidth: '0px',
          yestoday: this.getDate(+new Date() - 24 * 60 * 60 * 1000),
          windowHeight: document.body.clientHeight,
          windowWidth: document.body.clientWidth,
          dateSwiperOption: {
            initialSlide: 0,
            slidesPerView: 1,
            centeredSlides: false,
            autoplayDisableOnInteraction: false,
            autoplay: 2500,
            grabCursor: true,
            loop: true,
            paginationClickable: true,
            nextButton: '.swiper-button-next',
            prevButton: '.swiper-button-prev',
            spaceBetween: 30,
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
          currentLook: '',
          activeBg: 'activeBg',
          showUserOverview: false,
          UserOverviewHeight: 0,
          summary: {
            username: null,
            phone: null,
            lookcar: null,
            intention: null,
            sex: null,
            focus: [],
            color: [],
            voice_time: null,
            communicate_time: 0,
            budget: null,
          },
          dateList: [
            {
              name: '7月12日',
              value: '20170712',
            },
            {
              name: '7月13日',
              value: '20170713',
            },
          ],
          carList: [
            {
              name: '长安cs15',
              value: '1',
            },
            {
              name: '长安cs95',
              value: '2',
            },
          ],
          averageLookTime: null,
          mrate: null,
          wrate: null,
          totalUsers: null,
          currentOnlineUsers: null,
          carColorList: [],
          activeUser: [],
          purchaseList: [],
          purchaseUsers: [],
          carfoucus: [],
          colorSum: 0,
        }
      },
      computed: {
        communicate() {
          return this.summary.communicate_time > 0 ? this.secondsToMinutes(this.summary.communicate_time) : '--';
        },
        voiceTime() {
          return this.secondsToMinutes(this.summary.voice_time);
        },
        focusMostColor() {
          return this.summary.color.length > 0 ? this.summary.color[0] : '';
        },
        mostFocus() {
          return this.summary.focus.slice(0, 4);
        },
        sexTag() {
          return this.summary.sex === '男' ? '他' : '她';
        },
        perVw() {
          return this.windowWidth / 100;
        },
        // colorSum() {
        //   return _.reduce(carColorList, (o) => {
        //     return o[0];
        //   }, 0)
        // }
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
        }
      },
      mounted() {
        this.getUrlHash();
        // this.fecthOnlineDatas();
        const $this = this;
        // this.scatterWidth = this.$refs.scatter.offsetHeight * 0.98 * 487 / 973  + 30 + 'px';
        this.windowHeight = document.body.clientHeight;
        this.windowWidth = document.body.clientWidth;
        window.addEventListener('resize', this.handleResize);
        setInterval(function() {
          $this.totalUsers = $this.totalUsers + 1;
          $this.currentOnlineUsers = $this.currentOnlineUsers + 1;
        }, 2000);
        setTimeout(() => {
          $('.current-view').liMarquee({
            height: 50,
          });
        }, 1000)
        // this.handleShowOverviewInterval();
      },
      beforeDestroy() {
        window.removeEventListener('resize', this.handleResize);
      },
      methods: {
        getUrlHash() {
          const url = window.location.href;
          this.carId = url.match(/car=(\d+)/) ? parseInt(url.match(/car=(\d+)/)[1]) : '';
          this.fetchBasicDatas();
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
          this.scatterWidth = this.$refs.scatter.offsetHeight * 0.98 * 487 / 973  + 30 + 'px';
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
        setUserDetailPostion(top, target) {
          const height = $(".user-detail").height();
          if (top + height / 2 >= this.windowHeight) {
            this.$refs.userDetail.style.top = (this.windowHeight - height) / this.perVw + 'vw';
          } else if (top <= height / 2) {
            this.$refs.userDetail.style.top = 0;
          } else {
            this.$refs.userDetail.style.top = (top - height / 2) / this.perVw + 'vw';
          }
          this.showUserOverview = true;
          setTimeout(() => {
            this.handleHideOverview(target);
          }, 3000);
        },
        handleShowOverview(top, target) {
          target.querySelector(".single-user").style.background = '#f03635';
          const arrow = target.querySelector(".arrow");
          arrow.style.display = "block";
          this.setUserDetailPostion(top, target);
        },
        handleHideOverview(target) {
          target.querySelector(".single-user").style.background = '';
          const arrow = target.querySelector(".arrow");
          arrow.style.display = "none";
          this.showUserOverview = false;
        },
        fetchBasicDatas() {
          const $this = this;
          axios.get(`/dealer/factory/overview/${this.carId}?t=${+new Date()}`)
          .then(response => {
            $this.assignBasicDatas(response.data);
            setTimeout($this.fetchBasicDatas, 500000);
          })
          .catch(error => {
            console.log(error);
            setTimeout($this.fetchBasicDatas, 5000);
          });
        },
        fecthOnlineDatas() {
          const $this = this;
          axios.get(`/dealer/users/experience/report/${this.lookId}?t=${+new Date()}`)
          .then(response => {
            $this.assignOnlineDatas(response.data)
            setTimeout($this.fecthOnlineDatas, 5000);
          })
          .catch(error => {
            console.log(error);
            setTimeout($this.fecthOnlineDatas, 5000);
          });
        },

        assignBasicDatas(parsed) {
          if (parsed) {
            this.averageLookTime = parsed.avg_look_time;
            this.wrate = parsed.wrate;
            this.mrate = parsed.mrate;
            this.currentOnlineUsers = parsed.vr_num + Math.random * 100;
            this.totalUsers = parsed.total_users_num;
            this.carColorList = parsed.look_colors;
            this.colorSum = _.reduce(this.carColorList, (result, value, key) => {
              return result + value[0];
            }, 0);
            // this.carPreferenceList = parsed.dealer_car;
            this.activeUser = parsed.active_user_num.date;
            this.purchaseList = parsed.car_intention;
            this.carfoucus = _.sortBy(parsed.carfoucus, [function(data) {
              return -data[0];
            }]);
            this.orderPurchaseUsers(parsed.car_intention);
          }
        },
        assignOnlineDatas(parsed) {
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
        orderPurchaseUsers(purchase) {
          this.purchaseUsers = _.reverse(_.map(purchase, (purchaseObj) => {
            return _.join(_.values(purchaseObj), '')
          }));
        },
        getDate(val) {
          const date = new Date(val);
          return (date.getMonth() + 1) + '月' + date.getDate() + '日';
        }
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
$mainShadows: 0 2px 0 0 rgba(0, 0, 0, 0.3);

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
        @include wh(7.6vw, auto);
      }
    }
    .title {
      @include wh(86.1vw, 8.6vh);
      @include flex('row', 'flex-start', 'center');

      .company-message {
        @include wh(41vw, 8.6vh);
        @include flex('row', 'flex-start', 'center');
        padding-left: 1.1vw;
        font-size: 0.8vw;

        .sub-title {
          margin-left: 0.6vw;
          font-size: 0.6vw;
          color: #7d7f90;
        }
      }
      .scroll-message {
        @include wh(45.1vw, 8.6vh);
        @include flex('row', 'flex-start', 'center');

        .current-view {
          @include wh(17vw, 4.3vh);
          height: 4.3vh !important;
          line-height: 4.3vh !important;
          font-size: 0.7vw;
          margin-right: 0.77vw;
          background: #272e3c;
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
          @include wh(12.3vw, 7.3vh);
          @include flex('row', 'flex-start', 'center');
          @include bg('#353b4d');
          box-shadow: $mainShadows;

          &-bg {
            @include wh(3vw, 7.3vh);
            @include flex('row', 'center', 'center');
          }
          &-name {
            @include flex('column', 'center', 'flex-start');
          }
        }
        &-four-part {
          @include flex('column', 'center', 'center');
          @include wh(12.3vw, 17.7vh);
          @include bg('#2e3342');
          border-radius: 10px;
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
        @include borderradius('', '', 10px, 10px);
        box-shadow: 0 0 40px 0 rgba(0, 0, 0, 0.3);
      }
      .core-color {
        @include padding(5vh, 1vw, 3.2vh, 1vw);
        @include flex('column', 'space-between', 'flex-start');

        &-item {
          @include wh(100%, '');
          @include flex('row', 'flex-start', 'center');

          &-left {
            @include wh(70%, '');
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
              font-size: 0.7vw;
            }

          }
          &-right {
            @include wh(30%, '');
            @include flex('row', 'flex-end', 'center');

            .color-rate-num {
              font-size: 0.8vw;
              margin-right: 0.5vw;
            }
            .color-chart {
              @include wh(2vw, 2vw);
            }
          }
        }
      }
      .heat {
        @include wh(36.8vw, 44.6vh);
      }
      .today-look {
        @include wh(36.8vw, 37.5vh);
      }

      .purchase-color-chart-wrapper {
        @include flex('row', 'flex-start', 'flex-start');
      }

      .individuation {
        @include wh(38.8vw, 44.6vh);
      }
      .purchase {
        @include wh(18.8vw, 37.5vh);
      }
      .color-preference {
        @include wh(18.8vw, 37.5vh);
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
      }
      .current-online {
        @include wh(6.3vw, 74.1vh);
      }
    }
  }

  .core-mb {
    @include margin('', '', 1.9vh, '');
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
    font-size: 0.7vw;

    & > div {
      width: 100%;
    }
  }
  .swiper-button-red {
    background-image: none;
  }
  .swiper-button-prev,
  .swiper-button-next {
    height: 4.3vh !important;
    line-height: 4.3vh !important;
    font-size: 0.9vw;
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
    font-size: 2vw;

    &-sex {
      @include flex('row', 'space-between', 'center');
      @include wh(100%, auto);
    }
    .sex {
      @include wh(42%, auto);
      font-size: 1.5vw;
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
    font-size: 0.7vw;
  }
  .core-header {
    @include flex('row', 'flex-start', 'center');
    @include wh(100%, 7.33vh);
    @include bg('#353b4c');
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
  .core-content {

  }
  .core-content-top {
    @include wh(100%, 37.27vh);
  }
  .core-content-btm {
    @include wh(100%, 30.17vh);
  }
  .main-title {
    @include nopaddingmargin;
    font-size: 0.75vw;
  }
  .sub-title {
    @include nopaddingmargin;
    font-size: 0.5vw;
    color: #7d7f90;
  }
}
</style>
