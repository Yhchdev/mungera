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
        style="width: 30%;margin-right: 20px"
      >
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
      <el-button :icon="Search" type="primary" @click="searchFun">搜索</el-button>
    </div>
  </main>
</template>

<script setup>
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import { Search } from "@element-plus/icons-vue";
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

onMounted(() => {});
const getList = (param) => {
  axios
    .get(`http://192.168.1.6:9000/search?keyword=${param}`) // 替换为您的API端点
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

const searchFun=()=>{
  router.push({
    path: "/pic",
    query: { id: inputQ.value, quarter: quarter.value },
  });
}

</script>

<style lang="scss" scoped>
main {
  display: flex;
  justify-content: center;
  height: 100%;
  margin-top: 10%;
}
.search {
  width: 100%;
}
</style>