<template>
    <div :class="className" :listData="listData" :perVw="perVw" :id="id" :style="{height: height, width: width}"></div>
</template>
<script>
    import _ from 'lodash';

    // 引入 ECharts 主模块
    const echarts = require('echarts/lib/echarts');
    // 引入图
    require('echarts/lib/chart/bar');
    require('echarts/lib/component/title');
    require('echarts/lib/component/legend');
    export default {
      name: 'focusChart',
      props: {
        className: {
          type: String,
          default: 'focus-chart'
        },
        id: {
          type: String,
          default: 'focus-chart'
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
          this.setFocus(newList);
        },
        perVw(newVal) {
          this.perVw = newVal;
        },
      },
      mounted() {
        this.chart = echarts.init(document.getElementById(this.id));
        this.setFocus(this.listData);
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
        formatPercent(val, total) {
          let res = isNaN(val) ? 0 : val;
          if (isNaN(total) || total === 0) {
            res = 0;
          } else {
            res = res / total * 100;
          }
          return res === 0 ? 0 : res.toFixed(1) + '%';
        },
        setFocus(listData) {
          const $this = this;
          let category = [];
          let focusData = [];
          let allData = [];
          const len = listData.length;
          _.forEach(listData, (value, key) => {
            if ( key < 4 ) {
              category.push(value[1]);
              focusData.push(value[0]);
            }
            allData.push(value[0]);
          })
          const total = _.reduce(allData, (sum, n) => {
            return sum + n;
          }, 0)
          this.chart.setOption({
            title: {
              text: '客户关注占比',
              left: 'center',
              top: '5%',
              textStyle: {
                fontWeight: 'normal',
                fontSize: 0.7 * this.perVw,
                color: '#fff',
              },
              subtext: '看车零部件时长占比',
              subtextStyle: {
                fontWeight: 'normal',
                fontSize: 0.4 * this.perVw,
                color: '#7d7f90',
              },
            },
            legend: {
              show: false,
            },
            grid: {
              top: '10%',
              left: '5%',
              right: '5%',
              bottom: '5%',
              containLabel: true
            },
            yAxis: {
              show: false,
              type: 'value',
              boundaryGap: [0, 0.01]
            },
            xAxis: {
              type: 'category',
              data: category,
              splitLine: {
                show: false
              },
              axisLine: {
                show: false,
              },
              axisTick: {
                show: false
              },
              axisLabel: {
                margin: 1 * this.perVw,
                interval: 0,
                textStyle: {
                  fontSize: 0.6 * this.perVw,
                  color: '#b8b9c8',
                  baseline: 'middle',
                  align: 'center',
                }
              },
            },
            series: [
              {
                name: '用户重点关注',
                type: 'bar',
                barWidth: 15,
                itemStyle: {
                  normal: {
                    barBorderRadius: 2,
                    color: new echarts.graphic.LinearGradient(
                      0, 0, 1, 0,
                      [
                        {offset: 0, color: 'rgb(241, 73, 57)'},
                        {offset: 1, color: 'rgb(245, 123, 68)'}
                      ]
                    )
                  },
                  emphasis: {
                    show: false,
                  },
                },
                label: {
                  normal: {
                    show: true,
                    position: 'top',
                    formatter: function(a) {
                      return $this.formatPercent(a.data, total);
                    },
                    textStyle: {
                      color: "#fff",
                      fontSize: 0.65 * this.perVw,
                      fontWeight: 200,
                    }
                  },
                  emphasis: {
                    show: true,
                    position: 'top',
                    formatter: function(a) {
                      return $this.formatPercent(a.data, total);
                    },
                    textStyle: {
                      color: "#fff",
                      fontSize: 0.65 * this.perVw,
                      fontWeight: 200,
                    }
                  },
                },
                data: focusData
              }
            ]
          })
        }
      },
    }
</script>
