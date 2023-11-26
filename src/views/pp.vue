<template>
    <div class="about">
      <div class="bar">
        <div class="chart1" ref="chartDom1" :loading="loadingP"></div>
        <div class="chart2" ref="chartDom2" :loading="loadingP"></div>
        <div class="chart3" ref="chartDom3" :loading="loadingP"></div>
      </div>
    </div>
  </template>
  <script setup>
  import { ref, onMounted, computed } from "vue";
  import axios from "axios";
  import * as echarts from "echarts";
  import { useRoute } from "vue-router";
  const route = useRoute();
  const chartData = ref(null); // 用于存储获取到的数据
  const picIndex = computed(() => {
    return route.query.id;
  });
  const quarter = computed(() => {
    return route.query.quarter;
  });
  const loadingP = ref(false);
  onMounted(() => {
    fetchData();
  });
  const chartDom1 = ref(null);
  const chartDom2 = ref(null);
  const chartDom3 = ref(null);
  
  const fetchData = () => {
    loadingP.value = true;
    axios
      .get(
        `http://192.168.1.7:9000/chart?secucode=${picIndex.value}&report=${quarter.value}`
      ) // 替换为您的API端点
      .then((response) => {
        chartData.value = response.data; // 将获取到的数据存储到chartData中
        renderChart(); // 数据获取成功后调用渲染图表的方法
        loadingP.value = false;
      })
      .catch((error) => {
        console.error("Error:", error);
        loadingP.value = true;
      });
  };
  const renderChart = () => {
    // 使用Echarts渲染图表
    const chart1 = echarts.init(chartDom1.value); // 替换为您的图表容器ID
    const chart2 = echarts.init(chartDom2.value); // 替换为您的图表容器ID
    const chart3 = echarts.init(chartDom3.value); // 替换为您的图表容器ID
  
    // 使用chartData.value填充图表数据
    let option1 = {
      tooltip: {
        trigger: "axis",
        axisPointer: {
          type: "shadow",
        },
      },
      title: {
        text: chartData.value.charts[0].name,
        left: "8%",
      },
      legend: {
        data: [
          chartData.value.charts[0].series[0].name,
          chartData.value.charts[0].series[1].name,
        ],
      },
      xAxis: {
        type: "category",
        data: chartData.value.charts[0].series[0].x,
      },
      yAxis: [
        {
          name: '',
          type: "value",
        },
        {
          name: '',
          type: "value",
        },
      ],
      series: [
        {
          name: chartData.value.charts[0].series[0].name,
          data: chartData.value.charts[0].series[0].y,
          type: chartData.value.charts[0].series[0].type,
          yAxisIndex: "0", // 牛逼，共用x轴
        },
        {
          name: chartData.value.charts[0].series[1].name,
          data: chartData.value.charts[0].series[1].y,
          type: chartData.value.charts[0].series[1].type,
        },
      ],
    };
    let option2 = {
      tooltip: {
        trigger: "axis",
        axisPointer: {
          type: "shadow",
        },
      },
      title: {
        text: chartData.value.charts[1].name,
        left: "8%",
      },
      legend: {
        data: [
          chartData.value.charts[1].series[0].name,
          chartData.value.charts[1].series[1].name,
        ],
      },
      xAxis: {
        type: "category",
        data: chartData.value.charts[1].series[0].x,
      },
      yAxis: [
        {
          name: '',
          type: "value",
        },
        {
          name:'',
          type: "value",
        },
      ],
      series: [
        {
          name: chartData.value.charts[1].series[0].name,
          data: chartData.value.charts[1].series[0].y,
          type: chartData.value.charts[1].series[0].type,
          yAxisIndex: "0", // 牛逼，共用x轴
        },
        {
          name: chartData.value.charts[1].series[1].name,
          data: chartData.value.charts[1].series[1].y,
          type: chartData.value.charts[1].series[1].type,
        },
      ],
    };
    let option3 = {
      tooltip: {
        trigger: "axis",
        axisPointer: {
          type: "shadow",
        },
      },
      title: {
        text: chartData.value.charts[2].name,
        left: "8%",
      },
      legend: {
        data: [chartData.value.charts[2].series[0].name, chartData.value.charts[2].series[1].name,chartData.value.charts[2].series[2].name],
      },
      xAxis: {
        type: "category",
        data: chartData.value.charts[2].series[0].x,
      },
      yAxis: [
        {
          name: '',
          type: "value",
        },
        {
          name: '',
          type: "value",
        },
        {
          name: '',
          type: "value",
        },
      ],
      series: [
        {
          name: chartData.value.charts[2].series[0].name,
          data: chartData.value.charts[2].series[0].y,
          type: chartData.value.charts[2].series[0].type,
          yAxisIndex: "0", // 牛逼，共用x轴
          




          
        {
          name: chartData.value.charts[2].series[1].name,
          data: chartData.value.charts[2].series[1].y,
          type: chartData.value.charts[2].series[1].type,
        },
        {
          name: chartData.value.charts[2].series[2].name,
          data: chartData.value.charts[2].series[2].y,
          type: chartData.value.charts[2].series[2].type,
        },
      ],
    };
    chart1.setOption(option1);
    chart2.setOption(option2);
    chart3.setOption(option3);
  };
  </script>
  <style lang="scss" scoped>
  .chart1,
  .chart2,
  .chart3 {
    width: 1000px;
    height: 500px;
  }
  .about {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .bar {
    // overflow-y: scroll;
    // height: 100%;
    // width: 100%;
    // display: flex;
    // justify-content: center;
  }
  </style>