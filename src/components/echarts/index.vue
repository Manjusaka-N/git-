<template>
  <div class="container">
    <div class="title">五星月赛排名</div>
    <div class="barContainer" ref="sysqpm"></div>
  </div>
</template>
  
  <script>
import * as echarts from "echarts";

export default {
  name: "EvenMonthsRanking",
  data() {
    return {
      monthsData: [3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      rankingData: [],
      timerRun: null,
    };
  },
  async mounted() {
    let data = [];
    this.monthsData.forEach((i) => {
      data.push(`${i}月`);
    });
    this.monthsData = data;
    await this.initGetChartData();
    this.echartsLineInitsysqpm();
    this.runTimer();
  },
  methods: {
    runTimer() {
      this.timerRun = setInterval(() => {
        this.rankingData = [];
        setTimeout(async () => {
          await this.initGetChartData();
          this.echartsLineInitsysqpm();
        }, 200);
      }, 10 * 1000);
    },
    // 随时可以停下来
    cleanup() {
      if (this.timerRun) {
        clearInterval(this.timerRun);
        this.timerRun = null;
      }
    },
    async initGetChartData() {
      this.rankingData = [3, 4, 5];
    },
    echartsLineInitsysqpm() {
      if (this.$refs.sysqpm) {
        this.sysqpm = echarts.init(this.$refs.sysqpm);
        const options = {
          grid: {
            top: "8%",
            bottom: "15%",
            left: "5%",
            right: "5%",
          },
          xAxis: {
            type: "category",
            data: this.monthsData,
            axisTick: {
              show: false,
            },
            axisLabel: {
              color: "black",
              fontSize: 18,
              fontWeight: 500,
              interval: 0,
            },
          },
          yAxis: {
            type: "value",
            min: 2,
            max: 5,
            inverse: false,
            interval: 1,
            axisLine: {
              show: true,
            },
            axisLabel: {
              color: "black",
              fontSize: 18,
              fontWeight: 500,
              formatter: function (value) {
                if ((value === 2)) {
                  return 0;
                }
                const a = { 3: "三", 4: "四", 5: "五" };
                return a[value] + "星级";
              },
            },
            splitLine: {
              lineStyle: {
                show: false,
                width: 2,
                color: [
                  "rgba(255,255,255,.2)",
                  "rgba(255,255,255,.2)",
                  "rgba(255,255,255,.2)",
                  "rgba(255,255,255,.2)",
                  "rgba(255,255,255,.2)",
                  "rgba(255,255,255,.2)",
                ],
              },
            },
          },
          series: [
            {
              data: this.rankingData,
              type: "line",
              smooth: false,
              // symbol: 'none',
              lineStyle: {
                color: "#22ffcc",
              },
            },
          ],
        };
        this.sysqpm.clear();
        this.sysqpm.setOption(options);
      }
    },
  },
  beforeDestroy() {
    if (this.timerRun) {
      clearInterval(this.timerRun);
      this.timerRun = null;
    }
  },
};
</script>
  
    <style lang="scss" scoped>
.container {
  width: 100%;
  height: 285px;
  margin-bottom: 15px;
  color: white;

  .title {
    width: 100%;
    height: 45px;
    padding-left: 25px;
    font-size: 23px;
    font-weight: 500;
    line-height: 43px;
    background-image: url("~@/assets/images/DigtalManage/title.png");
    background-repeat: no-repeat;
  }

  .barContainer {
    height: 240px;
  }
}
</style>
  