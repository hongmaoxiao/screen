<template>
    <div class="screen-container">
      <transition name="slide-fade">
        <div class="user-detail" ref="userDetail" v-show="showUserOverview">
          <div class="user-detail-inner">
            <div class="user-detail-inner-title">
              用户体验报告
            </div>
            <div class="user-detail-inner-placeholder"></div>
            <div class="user-detail-inner-top">
              <div class="user-avatar">
                {{summary.username}}
              </div>
              <div class="user-info-overview">
                <p>{{summary.phone}}</p>
                <p class="user-info-intention">购买意向{{summary.intention}}%</p>
                <p class="font-weight-200">语言留言{{communicate}}分钟</p>
                <p class="font-weight-200">通话时长{{voiceTime}}分钟</p>
              </div>
              <div class="user-sex">
                <span class="sex-name">{{summary.sex}}士</span>
                <img class="sex-min" :src="summary.sex === '男' ? man : girl" alt="sex">
              </div>
            </div>
            <div class="user-detail-inner-mid">
              {{sexTag}}关注了{{summary.lookcar}}，预算{{summary.budget}}
              万，喜欢{{focusMostColor}}。
            </div>
            <div class="user-detail-inner-btm">
              <div class="user-focus-title">
                <img class="conmen" :src="conmen" alt="conmen">
                <span>重点关注</span>
              </div>
              <div class="user-focus-feature">
                <p v-for="focus in mostFocus">{{focus}}</p>
              </div>
            </div>
          </div>
        </div>
      </transition>
      <div class="wrapper-left">
        <div class="screen-left-top">
          <div class="statistics-overview-main">
            <p class="statistics-overview-title" @click="fullScreen()" id="full-screen">
              车势科技
            </p>
            <p class="sub-title">汽车行业智能销售专家</p>
          </div>
        </div>
        <div class="screen-left-btm">
          <div class="information-overview">
            <div class="information-overview-top
            ">
              <p class="overview-title">信息总览</p>
              <p class="overview-sub-title">当天用户数据</p>
            </div>
            <div class="information-overview-btm
            ">
              <div class="overview-one-of-four">
                <div class="overview-inner">
                  <p class="vr-total-num">
                    <i-odometer
                      :value="totalUsers"
                    ></i-odometer>人
                  </p>
                  <p class="vr-total-title">移动VR体验总人数</p>
                </div>
              </div>
              <div class="overview-one-of-four">
                <div class="overview-inner">
                  <p class="current-online-num">
                    <i-odometer
                      :value="currentOnlineUsers"
                    ></i-odometer>人
                  </p>
                  <p class="current-online-title">当前在线人数</p>
                </div>
              </div>
              <div class="overview-one-of-four">
                <div class="overview-inner">
                  <p class="average-look-car-time-num">
                    <i-odometer
                      :value="averageLookTime"
                    ></i-odometer>
                    <span class="average-look-car-time-unit">分钟</span>
                  </p>
                  <p class="average-look-car-time-title">平均看车时间</p>
                </div>
              </div>
              <div class="overview-one-of-four">
                <div class="overview-inner">
                  <div class="sex-main-rate">
                    <span class="sex-boy">
                      <img class="man sex-icon" :src="man" alt="man">
                      <i-odometer
                        :value="mrate"
                      ></i-odometer>
                      %
                    </span>
                    <span class="sex-girl">
                      <img class="girl sex-icon" :src="girl" alt="girl">
                      <i-odometer
                        :value="wrate"
                      ></i-odometer>
                      %
                    </span>
                  </div>
                  <div class="sex-main-title">男女占比</div>
                </div>
              </div>
            </div>
          </div>
          <div class="charts">
            <div class="attention-mid most-focus">
              <div class="most-focus-left divide-by-three">
                <div class="most-focus-left-overview attention-mid-top">
                  <p class="zh-name">
                    客户关注点热力图
                  </p>
                  <p class="en-name">
                    看车时长热力图
                  </p>
                </div>
                <div class="most-focus-left-main" ref="scatter">
                  <img class="car" :src="car" alt="car">
                  <div class="focus-scatter" :style="{ width: scatterWidth }">
                    <focusScatter :lastTenLook="lastTenLook" :perVw="perVw" :listData="carfoucus" height='100%' :width='scatterWidth' />
                  </div>
                </div>
                <div class="most-focus-left-placeholder"></div>
              </div>
              <div class="most-focus-mid divide-by-three">
                <div class="most-focus-mid-placeholder"></div>
                <div class="most-focus-rate-chart">
                  <focus :perVw="perVw" :listData="carfoucus" height='100%' width='100%' />
                </div>
              </div>
              <div class="most-focus-right divide-by-three-last">
                <div class="attention-mid-top">
                  <p class="zh-name">
                    车系偏好
                  </p>
                  <p class="en-name">
                    看车零部件时长占比
                  </p>
                </div>
                <div class="model-preference">
                  <carPreference :perVw="perVw" :listData="carPreferenceList" height='100%' width='100%' />
                </div>
                <div class="most-focus-left-placeholder"></div>
              </div>
            </div>
            <div class="attention-btm">
              <div class="color-preference divide-by-three">
                <div class="attention-btm-placeholder-top"></div>
                <div class="color-preference-main attention-btm-mid">
                  <div class="attention-btm-mid-overview">
                    <p class="zh-name">
                      个性化选配统计
                    </p>
                    <p class="en-name">
                      观看总时长和次数
                    </p>
                  </div>
                  <div class="attention-btm-mid-content color-rate">
                    <div class="color-wrapper">
                      <div class="color" v-for="color in lookColors">
                        <span class="color-bg" :style="{ background: color[2] }"></span>
                        <span>{{color[1]}}</span>
                        <span class="color-rate-num">{{color[0]}}%</span>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="attention-btm-placeholder-bottom"></div>
              </div>
              <div class="purchase-intention divide-by-three">
                <div class="attention-btm-placeholder-top"></div>
                <div class="purchase-intention-main attention-btm-mid">
                  <div class="attention-btm-mid-overview">
                    <p class="zh-name">
                      购买意向统计
                    </p>
                    <p class="en-name">
                      客户购买意向人数
                    </p>
                  </div>
                  <div class="attention-btm-mid-content">
                    <purchase :perVw="perVw" :listData="purchaseList" height='80%' width='100%' />
                    <div class="purchase-user">
                      <p v-for="purchase in purchaseUsers" class="divide-by-four">{{purchase}}人</p>
                    </div>
                  </div>
                </div>
                <div class="attention-btm-placeholder-bottom"></div>
              </div>
              <div class="active-user divide-by-three-last">
                <div class="attention-btm-placeholder-top"></div>
                <div class="active-user-main attention-btm-mid">
                  <div class="attention-btm-mid-overview">
                    <p class="zh-name">
                      当日看车人数
                    </p>
                    <p class="en-name">
                      当日看车人数
                    </p>
                  </div>
                  <div class="attention-btm-mid-content">
                    <active-user :perVw="perVw" :listData="activeUser" height='100%' width='100%' />
                  </div>
                </div>
                <div class="attention-btm-placeholder-bottom"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="wrapper-mid">
        <div class="grid-content screen-mid">
          <div class="attention-mid most-focus">
            <div class="most-focus-left divide-by-three">
              <div class="most-focus-left-overview attention-mid-top">
                <p class="zh-name">
                  客户关注点热力图
                </p>
                <p class="en-name">
                  看车时长热力图
                </p>
              </div>
              <div class="most-focus-left-main" ref="scatter">
                <img class="car" :src="car" alt="car">
                <div class="focus-scatter" :style="{ width: scatterWidth }">
                  <focusScatter :lastTenLook="lastTenLook" :perVw="perVw" :listData="carfoucus" height='100%' :width='scatterWidth' />
                </div>
              </div>
              <div class="most-focus-left-placeholder"></div>
            </div>
            <div class="most-focus-mid divide-by-three">
              <div class="most-focus-mid-placeholder"></div>
              <div class="most-focus-rate-chart">
                <focus :perVw="perVw" :listData="carfoucus" height='100%' width='100%' />
              </div>
            </div>
            <div class="most-focus-right divide-by-three-last">
              <div class="attention-mid-top">
                <p class="zh-name">
                  车系偏好
                </p>
                <p class="en-name">
                  看车零部件时长占比
                </p>
              </div>
              <div class="model-preference">
                <carPreference :perVw="perVw" :listData="carPreferenceList" height='100%' width='100%' />
              </div>
              <div class="most-focus-left-placeholder"></div>
            </div>
          </div>
          <div class="attention-btm">
            <div class="color-preference divide-by-three">
              <div class="attention-btm-placeholder-top"></div>
              <div class="color-preference-main attention-btm-mid">
                <div class="attention-btm-mid-overview">
                  <p class="zh-name">
                    个性化选配统计
                  </p>
                  <p class="en-name">
                    观看总时长和次数
                  </p>
                </div>
                <div class="attention-btm-mid-content color-rate">
                  <div class="color-wrapper">
                    <div class="color" v-for="color in lookColors">
                      <span class="color-bg" :style="{ background: color[2] }"></span>
                      <span>{{color[1]}}</span>
                      <span class="color-rate-num">{{color[0]}}%</span>
                    </div>
                  </div>
                </div>
              </div>
              <div class="attention-btm-placeholder-bottom"></div>
            </div>
            <div class="purchase-intention divide-by-three">
              <div class="attention-btm-placeholder-top"></div>
              <div class="purchase-intention-main attention-btm-mid">
                <div class="attention-btm-mid-overview">
                  <p class="zh-name">
                    购买意向统计
                  </p>
                  <p class="en-name">
                    客户购买意向人数
                  </p>
                </div>
                <div class="attention-btm-mid-content">
                  <purchase :perVw="perVw" :listData="purchaseList" height='80%' width='100%' />
                  <div class="purchase-user">
                    <p v-for="purchase in purchaseUsers" class="divide-by-four">{{purchase}}人</p>
                  </div>
                </div>
              </div>
              <div class="attention-btm-placeholder-bottom"></div>
            </div>
            <div class="active-user divide-by-three-last">
              <div class="attention-btm-placeholder-top"></div>
              <div class="active-user-main attention-btm-mid">
                <div class="attention-btm-mid-overview">
                  <p class="zh-name">
                    当日看车人数
                  </p>
                  <p class="en-name">
                    当日看车人数
                  </p>
                </div>
                <div class="attention-btm-mid-content">
                  <active-user :perVw="perVw" :listData="activeUser" height='100%' width='100%' />
                </div>
              </div>
              <div class="attention-btm-placeholder-bottom"></div>
            </div>
          </div>
        </div>
      </div> -->
      <div class="wrapper-right" :class="currentUsers.length === 0 ? activeBg : ''">
      </div>
      <div class="current-user">
        <swiper :options="swiperOption" ref="mySwiper">
          <swiper-slide
            v-for="user in currentUsers"
            key="user.id"
            :data-uid="user.id"
          >
            <div class="single-user">
              <p class="single-user-name-box">
                <span class="single-user-name">{{ user.username }}</span>
              </p>
              <i class="arrow left-arrow"></i>
            </div>
          </swiper-slide>
        </swiper>
        <div class="bg-holder" v-show="currentUsers.length"></div>
      </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import IOdometer from 'vue-odometer';
    import focus from './focus';
    import focusScatter from './focusScatter';
    import purchase from './purchase';
    import activeUser from './activeUser';
    import carPreference from './carPreference';
    import trend from './trend.png';
    import man from './man.png';
    import girl from './girl.png';
    import car from './car.png';
    import conmen from './conmen.png';

    axios.defaults.headers.common['Content-type'] = "application/json";

    export default {
      name: 'Screen',
      components: {
        focus,
        purchase,
        activeUser,
        carPreference,
        focusScatter,
        IOdometer,
      },
      data() {
        return {
          carId: 0,
          lookId: 0,
          currentUserId: 0,
          lastTenLook: [],
          showLook: false,
          trend: trend + '?' + +new Date(),
          man: man + '?' + +new Date(),
          girl: girl + '?' + +new Date(),
          car: car + '?' + +new Date(),
          conmen: conmen + '?' + +new Date(),
          top: 0,
          today: this.getDate(+new Date()),
          scatterWidth: '0px',
          yestoday: this.getDate(+new Date() - 24 * 60 * 60 * 1000),
          windowHeight: document.body.clientHeight,
          windowWidth: document.body.clientWidth,
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
          averageLookTime: null,
          mrate: null,
          wrate: null,
          totalUsers: null,
          currentOnlineUsers: null,
          lookColors: [],
          carPreferenceList: [],
          activeUser: [],
          purchaseList: [],
          purchaseUsers: [],
          carfoucus: [],
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
        this.fecthOnlineDatas();
        // this.fecthCurrentLookDatas();
        this.scatterWidth = this.$refs.scatter.offsetHeight * 0.98 * 487 / 973  + 30 + 'px';
        this.windowHeight = document.body.clientHeight;
        this.windowWidth = document.body.clientWidth;
        window.addEventListener('resize', this.handleResize);
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
            setTimeout($this.fetchBasicDatas, 5000);
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
            this.currentOnlineUsers = parsed.vr_num;
            this.totalUsers = parsed.total_users_num;
            this.lookColors = parsed.look_colors;
            this.carPreferenceList = parsed.dealer_car;
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

<style scoped>
.screen-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  color: #fff;
}
.screen-container input:-webkit-autofill {
  -webkit-box-shadow: 0 0 0px 1000px #293444 inset !important;
  -webkit-text-fill-color: #3E3E3E !important;
}
.screen-container p {
  margin: 0;
  padding: 0;
}
.screen-container > div {
  height: 100vh;
}
.screen-container .wrapper-left {
  width: 94.8vw;
}
.screen-container .screen-left-top {
  height: 14.5vh;
  background: #212531;
  border-bottom: 1px solid #3d465d;
  border-right: 1px solid #3d465d;
}
.screen-container .screen-left-btm {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  height: 85.5vh;
  background: #212531;
}
.screen-container .information-overview {
  width: 17.4vw;
  height: 100%;
  border-right: 1px solid #3d465d;
  position: relative;
}
.screen-container .information-overview-top {
  position: relative;
  height: 15%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.screen-container .information-overview-btm {
  height: 85%;
}
.screen-container .overview-one-of-four {
  height: 25%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}
.screen-container .overview-inner {
  width: 70%;
  height: 70%;
  display: flex;
  border: 1px solid #fff;
  border-radius: 6px;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}
.charts {
  width: 77.4vw;
  height: 100%;
  border-right: 1px solid #3d465d;
  position: relative;
}
.screen-container .wrapper-mid {
  width: 77.4vw;
  border: 0;
}
.screen-container .wrapper-right {
  background: #181923;
  width: 5.2vw;
}
.screen-container .activeBg {
  background: #2b303d;
}
.screen-container .grid-content {
  height: 100vh;
}
.screen-container .screen-mid {
  background: #181923;
}
.screen-container .left-top {
  height: 61.8vh;
  border-bottom: 1px solid #3d465d;
}
.screen-container .left-mid {
  height: 18.6vh;
  border-bottom: 1px solid #3d465d;
}
.screen-container .left-btm {
  height: 19.6vh;
}
.screen-container .overview {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.screen-container .overview-title {
  font-size: 1.35vw;
  padding: 0;
}
.screen-container .vr-total-num {
  font-size: 1.5vw;
}
.screen-container .vr-total-title {
  font-size: 0.8vw;
}

.screen-container .current-online-num {
  margin: 0;
  padding: 0;
  font-size: 1.5vw;
}
.screen-container .current-online-title {
  margin: 0;
  padding: 0;
  font-size: 0.8vw;
}
.screen-container .current-online-placeholder-btm {
  height: 25%;
}
.average-look-car-time-title {
  font-size: 0.8vw;
}
.average-look-car-time-num {
  font-size: 1.5vw;
  color: #dddddd;
}
.average-look-car-time-num .average-look-car-time-unit {
  display: inline-block;
  font-size: 1.5vw;
}
.screen-container .sex {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.screen-container .sex .sex-main {
  width: 100%;
  height: 52%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}
.sex-main-title {
  font-size: 0.8vw;
}
.sex-main-rate {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  font-weight: 200;
}
.sex-main-rate > span {
  width: 40%;
  height: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  font-size: 1.5vw;
}
.sex-boy {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  align-items: center;
}
.sex-girl {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
}
.sex-main-rate > span img {
  width: auto;
  height: 2vw;
  margin-right: 0.3vw;
}
.screen-container .sex .sex-placeholder-btm {
  height: 25%;
}
.screen-container .statistics-overview-main {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.screen-container .statistics-overview-main .statistics-overview-title {
  font-size: 1.6vw;
  font-weight: 900;
}
.screen-container .statistics-overview-main .sub-title {
  font-size: 0.71vw;
}
.screen-container .statistics-overview-main .statistics-overview-time {
  font-size: 0.92vw;
  color: #6f778e;
}
.screen-container .statistics-overview-main .statistics-overview-current {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  border-radius: 0.78vw;
  font-size: 0.92vw;
  height: 26%;
  padding: 0 1vw;
  background: #212531;
  color: #f03635;
}
.screen-container .attention-mid {
  width: 100%;
  height: 47.3%;
  border-bottom: 1px solid #3d465d;
  padding: 0 3.6% 0 4.6%;
}
.screen-container .attention-mid-top {
  width: 100%;
  height: 11.5%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-start;
}
.screen-container .most-focus {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
}
.screen-container .most-focus .most-focus-left {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
}
.screen-container .most-focus .most-focus-left .most-focus-left-main {
  position: relative;
  width: 80%;
  height: 79.9%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.screen-container .most-focus .most-focus-left .most-focus-left-main img {
  width: auto;
  height: 98%;
}
.screen-container .most-focus .most-focus-left .most-focus-left-main .focus-scatter {
  position: absolute;
  height: 98%;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.screen-container .most-focus .most-focus-mid {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.screen-container .most-focus .most-focus-mid .most-focus-mid-placeholder {
  width: 100%;
  height: 11.5%;
}
.screen-container .most-focus .most-focus-mid .most-focus-rate-chart {
  width: 100%;
  height: 88.5%;
}
.screen-container .most-focus .most-focus-right {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.screen-container .most-focus .most-focus-right .model-preference {
  width: 100%;
  height: 79.9%;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
}
.screen-container .most-focus .most-focus-left-placeholder {
  width: 100%;
  height: 8.6%;
}
.screen-container .attention-btm {
  width: 100%;
  height: 38.2%;
  padding: 0 3.6% 0 4.6%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: flex-start;
}
.screen-container .attention-btm > div {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
}
.screen-container .zh-name {
  font-size: 1.35vw;
}
.screen-container .en-name {
  font-size: 0.92vw;
  color: #58646e;
}
.screen-container .divide-by-three {
  width: 33.3%;
  height: 100%;
}
.screen-container .divide-by-four {
  width: 100%;
  height: 25%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.screen-container .divide-by-three-last {
  width: 33.4%;
  height: 100%;
}
.screen-container .attention-btm-mid {
  width: 100%;
  height: 78.8%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.screen-container .attention-btm-placeholder-top {
  width: 100%;
  height: 9.4%;
}
.screen-container .attention-btm-placeholder-btm {
  width: 100%;
  height: 11.8%;
}
.screen-container .attention-btm-mid-overview {
  width: 100%;
  height: 17.6%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-start;
}
.screen-container .attention-btm-mid-content {
  position: relative;
  width: 100%;
  height: 82.4%;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
}
.screen-container .color-rate {
  font-size: 1.05vw;
}
.screen-container .color-rate .color-wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-end;
  flex-wrap: wrap;
}
.screen-container .color-rate .color {
  width: 45%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  margin-right: 5%;
}
.screen-container .color-rate .color-bg {
  width: 1.17vw;
  height: 1.17vw;
}
.screen-container .color-rate .color-rate-num {
  font-weight: 200;
}
.screen-container .current-user {
  position: fixed;
  right: 0;
  top: 0;
  z-index: 9999;
  width: 5.786vw;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.screen-container .current-user .single-user {
  position: relative;
  margin-left: 0.586vw;
  width: 5.786vw;
  height: 5.786vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 0.9%;
}
.screen-container .current-user .single-user.active {
  background: #f03635;
}
.screen-container .current-user .single-user .single-user-name-box {
  margin-left: -0.568vw;
  width: 3.555vw;
  height: 3.555vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: green;
  border-radius: 50%;
}
.screen-container .current-user .single-user .single-user-name-box .single-user-name {
  font-size: 0.7vw;
  width: auto;
  max-width: 90%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.screen-container .current-user .bg-holder {
  width: 5.2vw;
  height: 100vh;
  background: #2b303d;
  position: fixed;
  top: 0;
  right: 0;
  z-index: -1;
}
.screen-container .swiper-slide {
  width: 100%;
}
.screen-container .user-detail {
  width: 17.74vw;
  height: 23.64vw;
  background: rgba(108, 111, 123, 0.7);
  position: fixed;
  right: 5.786vw;
  top: 0;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.user-detail {
  width: 17.74vw;
  height: 23.64vw;
  background: rgba(108, 111, 123, 0.7);
  position: fixed;
  right: 5.786vw;
  top: 0;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.user-detail .user-detail-inner {
  width: 16.36vw;
  height: 21.9vw;
  background: rgba(43, 48, 61, 0.9);
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.user-detail .user-detail-inner .user-detail-inner-title {
  width: 14.97vw;
  height: 3.11vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-bottom: 1px solid #9f9fc1;
  font-size: 1.6vw;
  font-weight: 900;
  color: #848da9;
}
.user-detail .user-detail-inner .user-detail-inner-placeholder {
  width: 13.84vw;
  height: 1.095vw;
}
.user-detail .user-detail-inner .user-detail-inner-top {
  width: 13.84vw;
  height: 6.37vw;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: flex-start;
}
.user-detail .user-detail-inner .user-detail-inner-top .user-avatar {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  width: 3.125vw;
  height: 3.125vw;
  font-size: 0.92vw;
  color: #fff;
  background: #f03635;
}
.user-detail .user-detail-inner .user-detail-inner-top .user-info-overview {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: flex-start;
  font-size: 0.74vw;
}
.user-detail .user-detail-inner .user-detail-inner-top .user-info-overview .user-info-intention {
  color: #dddddd;
}
.user-detail .user-detail-inner .user-detail-inner-top .user-sex {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.user-detail .user-detail-inner .user-detail-inner-top .user-sex .sex-name {
  margin-right: 0.39vw;
  color: #6f778e;
  font-size: 0.74vw;
}
.user-detail .user-detail-inner .user-detail-inner-top .user-sex .sex-min {
  width: 0.586vw;
  height: auto;
}
.user-detail .user-detail-inner .user-detail-inner-mid {
  width: 13.84vw;
  height: 3.847vw;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: flex-start;
  font-size: 0.92vw;
  font-weight: 700;
}
.user-detail .user-detail-inner .user-detail-inner-btm {
  width: 13.84vw;
  height: 7.478vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}
.user-detail .user-detail-inner .user-detail-inner-btm .user-focus-title {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
}
.user-detail .user-detail-inner .user-detail-inner-btm .user-focus-title .conmen {
  margin-right: 0.3125vw;
  width: 0.78vw;
  height: auto;
}
.user-detail .user-detail-inner .user-detail-inner-btm .user-focus-title span {
  font-size: 0.8vw;
}
.user-detail .user-detail-inner .user-detail-inner-btm .user-focus-feature {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  flex-wrap: wrap;
}
.user-detail .user-detail-inner .user-detail-inner-btm .user-focus-feature p {
  font-size: 0.92vw;
  border-radius: 0.586vw;
  background: #4f83f9;
  margin: 0.39vw 0.39vw 0 0;
  padding: 0.20vw 0.47vw;
  line-height: 1.0em;
}
.screen-container .font-weight-200 {
  font-weight: 200;
}
.screen-container .arrow {
  position: absolute;
  width: 0;
  height: 0;
  border-style: solid;
  border-color: transparent;
}
.screen-container .left-arrow {
  top: 50%;
  margin-top: -0.586vw;
  left: -1.172vw;
  border-width: 0.586vw 0.586vw 0.586vw 0.586vw;
  border-right-color: #f03635;
  display: none;
}
.screen-container .purchase-user {
  left: 0;
  top: 20%;
  font-size: 1.2vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  position: absolute;
  font-weight: 200;
  width: 60%;
  height: 80%;
}
.screen-container .slide-fade-enter {
  opacity: 0;
}
.screen-container .slide-fade-enter-active {
  opacity: 1;
}
.screen-container .slide-fade-enter-active {
  transition: all 1s linear;
}
.screen-container .slide-fade-leave-active {
  transition: all 1s cubic-bezier(1, 0.5, 0.8, 1);
}
.screen-container .slide-fade-leave-active {
  transform: translateX(17.74vw);
  opacity: 0;
}
.screen-container .slide-up-enter {
  opacity: 0;
}
.screen-container .slide-up-enter-active {
  opacity: 1;
}
.screen-container .slide-up-enter-active {
  transition: all 1s cubic-bezier(1, 0.5, 0.8, 1);
}
.screen-container .slide-up-leave-active {
  transition: all 1s cubic-bezier(1, 0.5, 0.8, 1);
}
.screen-container .slide-up-leave-active {
  transform: translateY(-1vw);
  opacity: 0;
}

.screen-container .odometer.odometer-auto-theme, .screen-container .odometer.odometer-theme-default {
  font-family: "Helvetica Neue", "Helvetica Neue", Helvetica, Arial, "Hiragino Sans GB", "Microsoft Yahei", sans-serif;
  font-weight: 200;
}
</style>
