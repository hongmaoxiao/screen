<template>
    <div :class="className" :listData="listData" :perVw="perVw" :id="id" :style="{height:height,width:width}"></div>
</template>
<script>
    import _ from 'lodash';

    // 引入 ECharts 主模块
    const echarts = require('echarts/lib/echarts');
    // 引入图
    require('echarts/lib/chart/gauge');
    export default {
      name: 'gaugeChart',
      props: {
        className: {
          type: String,
          default: 'purchase-chart'
        },
        id: {
          type: String,
          default: 'purchase-chart'
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
      watch: {
        listData(newList) {
          this.setPunnel(newList);
        },
        perVw(newVal) {
          this.perVw = newVal;
        },
      },
      data() {
        return {
          chart: null
        };
      },
      mounted() {
        this.chart = echarts.init(document.getElementById(this.id));
        this.setPunnel(this.listData);
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
        getAxisLineOption(map, key) {
          return {
            lineStyle: {
              width: 0.6 * this.perVw,
              color: [
                [map[key] ? map[key].rate : 0, map[key] ? map[key].color : "#FF4B4C"],
                [1, "#545a6c"],
              ]
            }
          };
        },
        setPunnel(listData) {
          const nameSize = 12 + 0.01 * this.perVw;
          const detailSize = 12 + 0.25 * this.perVw;
          const titleOption = {
            show: true,
            offsetCenter: [0, "25%"],
            textStyle: {
              color: "#7d7f90",
              fontSize: nameSize,
              fontFamily: "微软雅黑",
              fontWeight: "200"
            }
          };
          const detailOption = {
            formatter:'{value}',
            offsetCenter: [0, "-25%"],
            textStyle: {
              color: "#fff",
              fontSize: detailSize,
              fontFamily: "微软雅黑",
              fontWeight: "600"
            }
          };
          const names = {
            score_90: "90%以上",
            score_80_90: "80% ~ 90%",
            score_70_80: "70% ~ 80%",
            score_70: "70%以下",
          };
          const colors = {
            score_90: '#FF4B4C',
            score_80_90: '#E9D447',
            score_70_80: '#3AC667',
            score_70: '#37CEFD',
          };
          let purchaseMap = {};
          let purchaseSum = 0;
          purchaseSum = _.reduce(listData, (result, value, key) => {
            return result + +_.values(value);
          },  0);
          _.forEach(listData, (scoreObj) => {
            _.forOwn(scoreObj, (value, key) => {
              if (purchaseMap[key] === undefined) {
                purchaseMap[key] = {
                  name: names[key],
                  value,
                  color: colors[key],
                  rate: purchaseSum === 0 ? 0 : +(value / purchaseSum).toFixed(3),
                }
              }
            })
          })
          this.chart.setOption({
            calculable: true,
            series: [
              {
                name: '购买意向统计',
                startAngle: 180,
                endAngle: 0,
                type: 'gauge',
                center: ['25%', '35%'],
                radius: '40%',
                axisLine: this.getAxisLineOption(purchaseMap, 'score_90'),
                axisTick: {
                  show: false
                },
                axisLabel: {
                  show: false
                },
                splitLine: {
                  show: false
                },
                pointer: {
                  show: false,
                },
                title: titleOption,
                detail: detailOption,
                data: [
                  {
                    value: purchaseMap['score_90'] ? purchaseMap['score_90'].value : 0,
                    name: '90%以上',
                  }
                ]
              },
              {
                name: '购买意向统计',
                startAngle: 180,
                endAngle: 0,
                type: 'gauge',
                center: ['75%', '35%'],
                radius: '40%',
                axisLine: this.getAxisLineOption(purchaseMap, 'score_80_90'),
                axisTick: {
                  show: false
                },
                axisLabel: {
                  show: false
                },
                splitLine: {
                  show: false
                },
                pointer: {
                  show: false,
                },
                title: titleOption,
                detail: detailOption,
                data: [
                  {
                    value: purchaseMap['score_80_90'] ? purchaseMap['score_80_90'].value : 0,
                    name: '80% ~ 90%',
                  }
                ]
              },
              {
                name: '购买意向统计',
                startAngle: 180,
                endAngle: 0,
                type: 'gauge',
                center: ['25%', '80%'],
                radius: '40%',
                axisLine: this.getAxisLineOption(purchaseMap, 'score_70_80'),
                axisTick: {
                  show: false
                },
                axisLabel: {
                  show: false
                },
                splitLine: {
                  show: false
                },
                pointer: {
                  show: false,
                },
                title: titleOption,
                detail: detailOption,
                data: [
                  {
                    value: purchaseMap['score_70_80'] ? purchaseMap['score_70_80'].value : 0,
                    name: '70% ~ 80%',
                  }
                ]
              },
              {
                name: '购买意向统计',
                startAngle: 180,
                endAngle: 0,
                type: 'gauge',
                center: ['75%', '80%'],
                radius: '40%',
                axisLine: this.getAxisLineOption(purchaseMap, 'score_70'),
                axisTick: {
                  show: false
                },
                axisLabel: {
                  show: false
                },
                splitLine: {
                  show: false
                },
                pointer: {
                  show: false,
                },
                title: titleOption,
                detail: detailOption,
                data: [
                  {
                    value: purchaseMap['score_70'] ? purchaseMap['score_70'].value : 0,
                    name: '70%以下',
                  }
                ]
              },
            ]
          })
        }
      }
    }
</script>
