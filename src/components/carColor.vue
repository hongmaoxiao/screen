<template>
    <div :class="className" :id="id" :colorIndex="colorIndex" :perVw="perVw" :sum="sum" :listData="listData" :style="{height: height, width: width}"></div>
</template>
<script>
    import _ from 'lodash';

    // 引入 ECharts 主模块
    const echarts = require('echarts/lib/echarts');
    // 引入图
    require('echarts/lib/chart/pie');
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
        },
        sum: {
          type: Number,
          require: true
        },
        colorIndex: {
          type: Number,
          require: true,
        },
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
          const colors = [listData[this.colorIndex][2], '#545a6c'];
          carDatas.push({
            name: listData[this.colorIndex][1],
            value: listData[this.colorIndex][0],
          });
          carDatas.push({
            name: '其它',
            value: this.sum - listData[this.colorIndex][0] >= 0 ? this.sum - listData[this.colorIndex][0] : 0,
          });
          this.chart.setOption({
            color: colors,
            series : [
              {
                name: '颜色偏好统计',
                type: 'pie',
                radius : '55%',
                center: ['50%', '60%'],
                silent: true,
                data: carDatas,
                labelLine: {
                  normal: {
                    show: false,
                  },
                },
              }
            ]
          })
        }
      }
    }
</script>
