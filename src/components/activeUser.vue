<template>
    <div :class="className" :perVw="perVw" :listData="listData" :id="id" :style="{height: height, width: width}"></div>
</template>
<script>
    import _ from 'lodash';
    // 引入 ECharts 主模块
    const echarts = require('echarts/lib/echarts');
    // 引入图
    require('echarts/lib/chart/line');
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
        setActiveUser(listData) {
          let userDatas = [];
          let userCategory = [];
          let max = 0;
          let min = 0;
          let split = 5;
          _.forEach(listData, function(userObj) {
            _.forEach(userObj, function(user, time) {
              userCategory.push(time + ":00");
              userDatas.push(user);
            })
          });
          min = _.min(userDatas);
          max = _.max(userDatas);
          split = this.getGap(min, max);
          this.chart.setOption({
              color: ['#37b9fd'],
              grid: {
                left: '2%',
                right: '2%',
                bottom: '5%',
                containLabel: true
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
                      fontSize: 0.6 * this.perVw,
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
                    color: ['#fff'],
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
                    fontSize: 0.6 * this.perVw,
                    color: '#6f778e',
                  }
                },
              }],
              series: [
                {
                  name:'活跃用户',
                  type:'line',
                  smooth: true,
                  symbol: 'circle',
                  symbolSize: 5,
                  showSymbol: true,
                  lineStyle: {
                    normal: {
                      color: '#37b9fd',
                      width: 3,
                    },
                    emphasis: {
                      color: '#37b9fd',
                      width: 3,
                    },
                  },
                  itemStyle: {
                    normal: {
                      color: '#37b9fd',
                      borderColor: 'rgba(55, 185, 253, 0.2)',
                      borderWidth: 12
                    },
                    emphasis: {
                      color: '#37b9fd',
                      borderColor: 'rgba(55, 185, 253, 0.2)',
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
