<template>
  <div class="about">
    <PicSection v-for="(item,index) in chartData" :key="index+'pic'" :list='item' ref="chartDom1"  ></PicSection>
  </div>
</template>
<script setup>
import { ref, onMounted, computed } from "vue";
import PicSection from "../components/PicSection";
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

const fetchData = () => {
  loadingP.value = true;
  axios
    .get(
      `http://192.168.1.6:9000/chart?secucode=${picIndex.value}&report=${quarter.value}`
    ) // 替换为您的API端点
    .then((response) => {
      chartData.value = response.data.charts; // 将获取到的数据存储到chartData中
      console.log(chartData.value,'chartData.value');
      // renderChart(); // 数据获取成功后调用渲染图表的方法
      loadingP.value = false;
    })
    .catch((error) => {
      console.error("Error:", error);
      loadingP.value = true;
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
.chart1,
.chart2,
.chart3,
.chart4 {
  width: 100%;
  height: 500px;
}
</style>