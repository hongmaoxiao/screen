<template>
    <div :class="className" :perVw="perVw" :listData="listData" :id="id" :style="{height: height, width: width}"></div>
</template>
<script>
    import _ from 'lodash';
    // 引入 ECharts 主模块
    const echarts = require('echarts/lib/echarts');
    // 引入图
    require('echarts/lib/chart/line');
    require('echarts/lib/component/visualMap');
    require('echarts/lib/component/tooltip');
    export default {
      name: 'activeUserChart',
      props: {
        className: {
          type: String,
          default: 'active-user-chart'
        },
        id: {
          type: String,
          default: 'active-user-chart'
        },
        width: {
          type: String,
          default: '100%'
        },
        height: {
          type: String,
          default: '100%'
        },
        listData: {
          type: Array,
          require: true
        },
        perVw: {
          type: Number,
          require: true
        }
      },
      data() {
        return {
          chart: null
        };
      },
      watch: {
        listData(newList) {
          this.setActiveUser(newList);
        },
        perVw(newVal) {
          this.perVw = newVal;
        },
      },
      mounted() {
        this.chart = echarts.init(document.getElementById(this.id));
        this.setActiveUser(this.listData);
        window.addEventListener('resize', this.handleResize);
      },
      beforeDestroy() {
        window.removeEventListener('resize', this.handleResize);
      },
      methods: {
        handleResize() {
          if (this.chart) {
            this.chart.resize();
          }
        },
        getGap(min, max) {
          const gap = max - min;
          let split = 5;
          if (gap <= 10) {
            split = gap;
          } else if (gap % 2 === 0) {
            split = gap / 2;
          } else if (gap % 3 === 0) {
            split = gap / 3;
          } else if (gap % 5 === 0) {
            split = gap / 5;
          }
          if (split >= 10) {
            split = 10;
          }
          return split;
        },
        getCategoryDatas(datas) {
          const $this = this;
          let res = [];
          for (let i = 0, len = datas.timeArr.length; i < len; i++) {
            if (i < datas.cut) {
              res.push({
                value: $this.parseCategoryTime(datas.timeArr[i]),
                textStyle: {
                  color: datas.normalColor
                }
              })
            } else if (i === datas.cut) {
              res.push({
                value: `${$this.parseCategoryTime(datas.timeArr[i])}\n${$this.parseSplitTime(new Date())}`,
                textStyle: {
                  color: datas.activeColor
                }
              })
            } else {
              res.push({
                value: $this.parseCategoryTime(datas.timeArr[i]),
                textStyle: {
                  color: datas.activeColor
                }
              })
            }
          }

          return res;
        },
        parseCategoryTime(time) {
          return `${time}:00`;
        },
        parseSplitTime(val) {
          const date = new Date(val);
          const month = date.getMonth() + 1 < 10 ? `0${date.getMonth() + 1}` : date.getMonth() + 1;
          const day = date.getDate() < 10 ? `0${date.getDate()}` : date.getDate();
          return `${month}-${day}`
        },
        setActiveUser(listData) {
          const normalColor = '#7D7F90';
          const activeColor = '#37CEFD';
          let userDatas = [];
          let userCategory = [];
          let timeArr = [];
          let timeArrLen;
          let max = 0;
          let min = 0;
          let split = 5;
          const nameSize = 12 + 0.01 * this.perVw;
          _.forEach(listData, function(userObj) {
            _.forEach(userObj, function(user, time) {
              timeArr.push(+time);
              userDatas.push(user);
            })
          });
          min = _.min(userDatas);
          max = _.max(userDatas);

          split = this.getGap(min, max);
          const cut = _.findIndex(timeArr, (o) => {
            return o < 3;
          })
          timeArrLen = timeArr.length;
          userCategory = this.getCategoryDatas({
            timeArr,
            cut,
            normalColor,
            activeColor,
          });
          this.chart.setOption({
              grid: {
                left: '2%',
                right: '2%',
                bottom: '5%',
                containLabel: true
              },
              tooltip: {
                show: true,
                trigger: 'item',
                position: 'top',
                backgroundColor: 'transparent',
                borderColor: 'transparent',
                formatter: function(a) {
                  return `${a.value}人`;
                },
                textStyle: {
                  color: '#fff',
                }
              },
              xAxis: [
                {
                  type: 'category',
                  boundaryGap: true,
                  data: userCategory,
                  axisLine: {
                    show: false,
                  },
                  axisTick: {
                    show: false,
                  },
                  axisLabel: {
                    margin: 10,
                    textStyle: {
                      fontSize: nameSize,
                      color: '#6f778e',
                    }
                  },
                  splitArea: {
                    interval: 'auto',
                    show: false,
                    areaStyle: {
                      color: ['rgba(31, 34, 45, 0.5)', 'rgba(31, 34, 45, 1)']
                    }
                  }
                },
              ],
              yAxis: [{
                type: 'value',
                minInterval: 1,
                splitNumber: split,
                min: min,
                max: max,
                splitLine: {
                  show: true,
                  interval: 'auto',
                  lineStyle: {
                    color: ['#353b4c'],
                    width: 1,
                  },
                },
                axisLine: {
                  show: false,
                },
                axisTick: {
                  show: false
                },
                axisLabel: {
                  margin: 10,
                  textStyle: {
                    fontSize: nameSize,
                    color: '#6f778e',
                  }
                },
              }],
              visualMap: {
                show: false,
                dimension: 0,
                pieces: [{
                  lt: cut,
                  color: normalColor
                }, {
                  gte: cut,
                  lte: timeArrLen,
                  color: activeColor
                }]
              },
              series: [
                {
                  name:'活跃用户',
                  type:'line',
                  smooth: true,
                  symbol: 'circle',
                  symbolSize: 10,
                  showSymbol: true,
                  lineStyle: {
                    normal: {
                      width: 3,
                    },
                    emphasis: {
                      width: 3,
                    },
                  },
                  itemStyle: {
                    normal: {
                      borderWidth: 12
                    },
                    emphasis: {
                      borderWidth: 12
                    },
                  },
                  data: userDatas
                },
              ]
          })
        }
      }
    }
</script>
