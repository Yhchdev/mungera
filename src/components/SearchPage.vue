<template>
  <main>
    <div class="search">
      <el-select
        v-model="quarter"
        value-key="value"
        placeholder="Select"
        style="width: 100px; margin-right: 20px"
      >
        <el-option
          v-for="item in quarterList"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
      <el-select
        v-model="inputQ"
        filterable
        remote
        reserve-keyword
        placeholder="请输代码、名称或拼音缩写"
        :remote-method="remoteMethod"
        :loading="loading"
        style="width: 30%; margin-right: 20px"
      >
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
      <el-button :icon="Search" type="primary" @click="searchFun"
        >搜索</el-button
      >
    </div>
    <div class="hot">
      <p>热门股票</p>
      <span v-for="(item, index) in hotList" :key="index" @click="click(item)">{{
        item.label
      }} </span>
    </div>
  </main>
</template>

<script setup>
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import { Search } from "@element-plus/icons-vue";
import { hostname } from "../../env";

const router = useRouter();
const options = ref([]);
const inputQ = ref();
const loading = ref(false);
const quarterList = ref([
  { value: "一季报", label: "一季报" },
  { value: "中报", label: "中报" },
  { value: "三季报", label: "三季报" },
  { value: "年报", label: "年报" },
]);
const quarter = ref("年报");

onMounted(() => {
  getHot();
});
const hotList = ref([]);
const getHot = () => {
  axios
    .get(`${hostname}/hot_stock`) // 替换为您的API端点
    .then((res) => {
      hotList.value = res.data.stocks;
      console.log(hotList.value, "ddddddddd");
    })
    .catch((error) => {
      console.error("Error:", error);
    });
};
const getList = (param) => {
  axios
    .get(`${hostname}/search?keyword=${param}`) // 替换为您的API端点
    .then((res) => {
      options.value = res.data.stocks;
    })
    .catch((error) => {
      console.error("Error:", error);
    });
};
const remoteMethod = (query) => {
  if (query) {
    loading.value = true;
    setTimeout(() => {
      loading.value = false;
      getList(query);
    }, 200);
  } else {
    options.value = [];
  }
};

const searchFun = () => {
  if (!inputQ.value) return;
  router.push({
    path: "/pic",
    query: { id: inputQ.value, quarter: quarter.value },
  });
};
const click=(i)=>{
  router.push({
    path: "/pic",
    query: { id: i.value, quarter:'年报' },
  });
}
</script>

<style lang="scss" scoped>
main {
  width: 50%;
  // height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.search {
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding-top: 100px;
  // margin-top: 10%;
}
.hot {
  height: 10%;
  // width: 50%;
  p {
    height: 40px;
    line-height: 40px;
  }
  span {
    display: inline-block;
    padding: 5px 10px;
    cursor: pointer;
  }
}
</style>