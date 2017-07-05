<template>
    <div :class="className" :listData="listData" :perVw="perVw" :id="id" :style="{height:height,width:width}"></div>
</template>
<script>
    import _ from 'lodash';

    // 引入 ECharts 主模块
    const echarts = require('echarts/lib/echarts');
    // 引入图
    require('echarts/lib/chart/funnel');
    export default {
      name: 'funnelChart',
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
        setPunnel(listData) {
          const nameSize = 1.05 * this.perVw;
          const names = {
            score_90: "90%以上",
            score_80_90: "80% ~ 90%",
            score_70_80: "70% ~ 80%",
            score_70: "70%以下",
          };
          const mapData = _.map(listData, (scoreObj) => {
            let res = {};
            _.forOwn(scoreObj, (value, key) => {
              res.name = names[key];
              res.value = value;
            })
            return res;
          })
          this.chart.setOption({
            color: ['#f5b34b', '#f58142', '#f04544', '#d80a08'],
            calculable: true,
            series: [
              {
                name:'购买意向分配',
                type:'funnel',
                left: '0%',
                top: '0%',
                //x2: 80,
                bottom: 0,
                width: '60%',
                // height: {totalHeight} - y - y2,
                minSize: '0%',
                maxSize: '100%',
                sort: 'none',
                // sort: function(a, b) {
                //   console.log("a: ", a, b);
                //   return b - a;
                // },
                gap: 0,
                label: {
                  normal: {
                    show: true,
                    position: 'outside',
                    formatter: '{b}',
                    textStyle: {
                      color: '#fff',
                      fontSize: nameSize,
                      fontWeight: 200,
                    }
                  },
                  emphasis: {
                    show: true,
                    position: 'outside',
                    formatter: '{b}',
                    textStyle: {
                      color: '#fff',
                      fontSize: nameSize,
                      fontWeight: 200,
                    }
                  },
                },
                labelLine: {
                  normal: {
                    show: true,
                    length: 5 * this.perVw,
                  },
                  emphasis: {
                    show: true,
                    length: 5 * this.perVw,
                  },
                },
                data: mapData.reverse()
              }
            ]
          })
        }
      }
    }
</script>
