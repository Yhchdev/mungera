<template>
  <div class="bar">
    <b class="title">{{ chartData.name }}</b>
    <div class="chart" ref="chartDom" :loading="loadingP"></div>
  </div>
</template>
  <script setup>
import { ref, onMounted, computed, defineProps, watch } from "vue";
import axios from "axios";
import * as echarts from "echarts";
import { useRoute } from "vue-router";
const route = useRoute();
const loadingP = ref(false);
const chartDom = ref(null);
const title1 = ref();
const props = defineProps({
  list: null,
  xtitle: null,
});
const chartData = computed(() => {
  return props.list;
});

watch(
  () => props.list,
  (val) => {
    if (val) {
      setTimeout(() => {
        renderChart();
      }, 1000);
    }
  },
  { immediate: true }
);
const renderChart = () => {
  // 使用Echarts渲染图表
  const chart = echarts.init(chartDom.value); // 替换为您的图表容器ID
  title1.value = chartData.value.name;

  // 使用chartData.value填充图表数据
  let option1 = {
    tooltip: {
      trigger: "axis",
      axisPointer: {
        type: "shadow",
      },
    },
    // title: {
    //   text: chartData.value.name,
    //   left: "8%",
    // },
    legend: {
      data: (() => {
        let data = [];
        chartData.value.series.forEach((i) => {
          data.push(i.name);
        });
        return data;
      })(),
    },
    xAxis: {
      type: "category",
      data: props.xtitle,
    },
    yAxis: [
      {
        type: (() => {
          let type;
          chartData.value.series.forEach((i) => {
            type = i.YType;
          });
          return type;
        })(),
      },
    ],
    series: (() => {
      let series = [];
      chartData.value.series.forEach((i) => {
        series.push({
          name: i.name,
          data: i.y,
          type: i.type,
          yAxisIndex: "0", // 牛逼，共用x轴
          stack: i.stack,
          emphasis: i.emphasis,
        });
      });
      return series;
    })(),
  };

  chart.setOption(option1);

  window.addEventListener("resize", function () {
    chart.resize();
  });
};
</script>
  <style lang="scss" scoped>
.about {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.title {
  width: 100%;
  background: #eff4fa;
  height: 50px;
  line-height: 50px;
  margin-bottom: 20px;
}
.bar {
  // height: 100%;
  width: 80%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.chart
 {
  width: 100%;
  height: 500px;
}
</style>