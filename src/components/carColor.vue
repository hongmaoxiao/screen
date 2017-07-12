<template>
    <div :class="className" :id="id" :perVw="perVw" :listData="listData" :style="{height: height, width: width}"></div>
</template>
<script>
    import _ from 'lodash';

    // 引入 ECharts 主模块
    const echarts = require('echarts/lib/echarts');
    // 引入图
    require('echarts/lib/chart/bar');
    // 引入提示框和标题组件
    export default {
      name: 'carPreferenceChart',
      props: {
        className: {
          type: String,
          default: 'car-preference-chart'
        },
        id: {
          type: String,
          default: 'car-preference-chart'
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
          this.setCarPreference(newList);
        },
        perVw(newVal) {
          this.perVw = newVal;
        },
      },
      mounted() {
        this.chart = echarts.init(document.getElementById(this.id));
        this.setCarPreference(this.listData);
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
        half(val) {
          const half = Math.floor(val.length / 2);
          return val.slice(0, half) + '\n\n' + val.slice(half);
        },
        setCarPreference(listData) {
          const $this = this;
          let carDatas = [];
          let carCategory = [];
          let colors = [];
          for (var i = 0, len = listData.length; i < len; i++) {
            colors.push(listData[i][2])
            carDatas.push(listData[i][0]);
            carCategory.push(listData[i][1]);
          }
          const total = _.reduce(carDatas, (sum, n) => {
            return sum + n;
          }, 0)
          this.chart.setOption({
            color: colors,
            grid: {
              left: '0%',
              right: '0%',
              bottom: '0%',
              containLabel: true
            },
            xAxis: [{
              type: 'category',
              boundaryGap: true,
              nameGap: 150,
              data : carCategory,
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
                  fontSize: 0.7 * this.perVw,
                  color: '#fff',
                  baseline: 'top',
                },
                formatter: function(value) {
                  if (value.length > 8) {
                    return $this.half(value);
                  }
                  return value;
                },
              },
              splitArea: {
                interval: 'auto',
                show: true,
                areaStyle: {
                  color: ['rgba(31, 34, 45, 1)', 'rgba(31, 34, 45, 0.5)']
                }
              }
            }],
            yAxis: {
              show: false,
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
                textStyle: {
                  fontSize: 0.88 * this.perVw,
                  color: '#6f778e',
                }
              },
            },
            series: [{
              name: '车系偏好',
              type: 'bar',
              barWidth: 10,
              itemStyle: {
                normal: {
                  barBorderRadius: 10,
                  color: new echarts.graphic.LinearGradient(
                    0, 0, 0, 1,
                    [
                      {offset: 0, color: 'rgb(107, 75, 160)'},
                      {offset: 1, color: 'rgb(74, 67, 154)'}
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
                  position: 'top',
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
              data: carDatas
            },]
          })
        }
      }
    }
</script>
