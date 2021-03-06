<template>
    <div :class="className" :lastTenLook="lastTenLook" :listData="listData" :perVw="perVw" :id="id" :style="{height: height, width: width}"></div>
</template>
<script>
    import _ from 'lodash';

    // 引入 ECharts 主模块
    const echarts = require('echarts/lib/echarts');
    // 引入图
    require('echarts/lib/chart/scatter');
    require('echarts/lib/chart/effectScatter');
    // require('echarts/lib/component/tooltip');
    export default {
      name: 'focusScatterChart',
      props: {
        className: {
          type: String,
          default: 'focus-scatter-chart'
        },
        id: {
          type: String,
          default: 'focus-scatter-chart'
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
        },
        lastTenLook: {
          type: Array,
          require: true
        },
      },
      data() {
        return {
          chart: null,
          positionMap: {
            '仪表盘': {
              x: 110,
              y: 300,
            },
            '左前门': {
              x: 50,
              y: 220,
            },
            '空调区': {
              x: 140,
              y: 280,
            },
            '副驾驶位': {
              x: 168,
              y: 200,
            },
            '发动机盖': {
              x: 140,
              y: 400,
            },
            '右后门': {
              x: 235,
              y: 110,
            },
            '右前门': {
              x: 235,
              y: 220,
            },
            '左后门': {
              x: 40,
              y: 110,
            },
            '前格栅': {
              x: 140,
              y: 435,
            },
            '轮毂': {
              x: 50,
              y: 320,
            },
            '后备箱': {
              x: 140,
              y: 40,
            },
            '变速档位': {
              x: 140,
              y: 240,
            },
            '车窗': {
              x: 220,
              y: 200,
            },
            '后保险杠': {
              x: 140,
              y: 15,
            },
            '后座': {
              x: 168,
              y: 142,
            },
            '天窗': {
              x: 140,
              y: 180,
            },
            '多媒体区': {
              x: 140,
              y: 300,
            },
            '手扶箱': {
              x: 140,
              y: 210,
            },
            '前大灯': {
              x: 220,
              y: 425,
            },
            '刹车灯': {
              x: 75,
              y: 25,
            },
            '方向盘': {
              x: 120,
              y: 250,
            },
            '后排气': {
              x: 220,
              y: 25,
            },
            '车内顶部': {
              x: 180,
              y: 180,
            },
            '隐私玻璃': {
              x: 230,
              y: 250,
            },
            '18寸轮毂': {
              x: 230,
              y: 90,
            },
            '19寸轮毂': {
              x: 50,
              y: 90,
            },
          }
        };
      },
      mounted() {
        window.addEventListener('resize', this.handleResize);
      },
      watch: {
        listData(newList, oldList) {
          if (!_.isEqual(newList, oldList)) {
            this.render(newList, this.lastTenLook);
          }
        },
        lastTenLook(newLastTenLook) {
          this.render(this.listData, newLastTenLook);
        },
        perVw(newVal) {
          this.perVw = newVal;
        },
        width(newWidth) {
          this.width = newWidth;
          if (this.chart) {
            this.chart.resize({
              width: newWidth,
            })
          }
        }
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
        render(newList, newLastTenLook) {
          this.chart = echarts.init(document.getElementById(this.id));
          this.setFocusScatter(newList, newLastTenLook);
        },
        mapTenLook(tenLook, max) {
          let res = [];
          _.forEach(tenLook, (value, key) => {
            const pos = this.positionMap[value];
            if (pos && pos.x && pos.y) {
              res.push([pos.x, pos.y, Math.floor(Math.random() * max), value]);
            }
          })
          return res;
        },
        setFocusScatter(listData, lastTenLook) {
          const $this = this;
          let focusScatterData = [];
          let topFocusScatterData = [];
          const max = listData[0] ? listData[0][0] : 100;
          topFocusScatterData = this.mapTenLook(_.uniq(lastTenLook), max);
          const labelSize = 12 + 0.1 * this.perVw;
          _.forEach(listData, (value, key) => {
            const pos = $this.positionMap[value[1]];
            if (pos && pos.x && pos.y) {
              focusScatterData.push([pos.x, pos.y, value[0], value[1]]);
            }
          })
          const itemStyle = {
            normal: {
              opacity: 0.8,
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowOffsetY: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            },
          };
          this.chart.setOption({
            color: [
              '#f03635'
            ],
            grid: {
              x: 0,
              x2: '0%',
              y: '0%',
              y2: 0
            },
            xAxis: {
              show: true,
              type: 'value',
              gridIndex: 0,
              min: 0,
              max: 280,
              nameGap: 16,
              nameTextStyle: {
                color: '#fff',
                fontSize: 14
              },
              splitLine: {
                show: false
              },
              axisLine: {
                lineStyle: {
                  color: '#eee'
                }
              }
            },
            yAxis: {
              show: false,
              gridIndex: 0,
              min: 0,
              max: 460,
              type: 'value',
              nameLocation: 'end',
              nameGap: 20,
              nameTextStyle: {
                color: '#fff',
                fontSize: 16
              },
              axisLine: {
                lineStyle: {
                  color: '#eee'
                }
              },
              splitLine: {
                show: false
              }
            },
            series: [
              {
                name: '用户关注重点',
                type: 'scatter',
                symbolSize: function (val) {
                  return val[2] / 4;
                },
                itemStyle: itemStyle,
                data: focusScatterData
              },
              {
                name: '用户关注重点Top4',
                type: 'effectScatter',
                showEffectOn: 'render',
                symbolSize: function (val) {
                  return val[2] / 4;
                },
                rippleEffect: {
                  brushType: 'stroke'
                },
                hoverAnimation: true,
                label: {
                  normal: {
                    formatter: function(res) {
                      return res.data[3];
                    },
                    offset: [0, -5],
                    textStyle: {
                      color: "#fff",
                      fontSize: labelSize,
                    },
                    position: 'top',
                    show: true
                  },
                },
                itemStyle: itemStyle,
                data: topFocusScatterData
              }
            ]
          })
        }
      },
    }
</script>
