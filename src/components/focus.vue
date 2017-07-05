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
        setFocus(listData) {
          let category = [];
          let focusData = [];
          let allData = [];
          const len = listData.length;
          const newList = _.sortBy(listData, [function(data) {
            return data[0];
          }]);
          _.forEach(newList, (value, key) => {
            if ( key >= len - 4 ) {
              category.push(value[1]);
              focusData.push(value[0]);
            }
            allData.push(value[0]);
          })
          const total = _.reduce(allData, (sum, n) => {
            return sum + n;
          }, 0)
          this.chart.setOption({
            background: 'rgba(31, 34, 45, 1)',
            title: {
              text: '用户关注重点占比',
              left: 0,
              top: 0,
              textStyle: {
                fontWeight: 'normal',
                fontSize: 1.14 * this.perVw,
                color: 'rgba(111, 119, 142, 1)',
              },
            },
            legend: {
              show: false,
            },
            grid: {
              left: '0%',
              right: '10%',
              bottom: '0%',
              containLabel: true
            },
            xAxis: {
              show: false,
              type: 'value',
              boundaryGap: [0, 0.01]
            },
            yAxis: {
              offset: 2.5 * this.perVw,
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
                margin: 10,
                interval: 0,
                textStyle: {
                  fontSize: 0.88 * this.perVw,
                  color: '#fff',
                  baseline: 'middle',
                  align: 'left',
                }
              },
            },
            series: [
              {
                name: '用户重点关注',
                type: 'bar',
                barWidth: 10,
                itemStyle: {
                  normal: {
                    barBorderRadius: 10,
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
                    position: 'right',
                    offset: [0.8 * this.perVw, 0],
                    formatter: function(a) {
                      const val = a.value / total * 100;
                      return val.toFixed(1) + '%';
                    },
                    textStyle: {
                      color: "#fff",
                      fontSize: 0.88 * this.perVw,
                      fontWeight: 200,
                    }
                  },
                  emphasis: {
                    show: true,
                    position: 'right',
                    formatter: function(a) {
                      const val = a.value / total * 100;
                      return val.toFixed(1) + '%';
                    },
                    textStyle: {
                      color: "#fff",
                      fontSize: 0.88 * this.perVw,
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
