<template>
  <div class="about" ref="aboutDom" @scroll="handleScroll" >
    <PicSection
      v-for="(item, index) in chartData"
      :key="index + 'pic'"
      :list="item"
      :xtitle="xtitle"
    ></PicSection>
  </div>
  <TipPage @updateVisible="isshowTip" :showTip="showTip"></TipPage>
</template>
<script setup>
import { ref, onMounted, computed, onUnmounted } from "vue";
import PicSection from "../components/PicSection";
import TipPage from "../components/TipPage";

import axios from "axios";
import { hostname } from "../../env";

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
const ifSee = ref(false);
onMounted(() => {
  fetchData();
  //推荐下面这种获取方法,避免了查询不存在的存储返回null
 
});
const showTip = ref(false);
const isshowTip = (val) => {
  showTip.value = val;
};
const aboutDom = ref();
const handleScroll = () => {
  ifSee.value = localStorage.getItem("userFlag") ;
  if (aboutDom.value.scrollTop > 2000 && !ifSee.value) {
    isshowTip(true);
  }
};
const xtitle = ref(null);
const fetchData = () => {
  loadingP.value = true;
  axios
    .get(`${hostname}/chart?secucode=${picIndex.value}&report=${quarter.value}`) // 替换为您的API端点
    .then((response) => {
      chartData.value = response.data.stock_charts.charts; // 将获取到的数据存储到chartData中
      xtitle.value = response.data.stock_charts.x;
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
  overflow-y: scroll;
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