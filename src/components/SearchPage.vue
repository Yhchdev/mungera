<template>
  <div class="page">
    <main>
      <div class="icon">
        <img src="../assets/icon.jpg" alt="" />
      </div>
      <div class="search">
        <el-select
          v-model="quarter"
          value-key="value"
          placeholder="Select"
          class="selectcss"
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
          class="selectcss2"
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
        <h3>热门股票</h3>
        <span v-for="(item, index) in hotList" :key="index" @click="click(item)"
          >{{ item.label }}
        </span>
      </div>
    </main>
    <div class="footer">
      <a href="https://beian.miit.gov.cn/">滇ICP备19002959号-2</a>
      <a class="call" href="https://xueqiu.com/u/1346959439">
        <span>联系识牛股</span>
        <div class="img"><img src="../assets/snow.png" alt="" /></div>
      </a>
    </div>
  </div>
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
const click = (i) => {
  router.push({
    path: "/pic",
    query: { id: i.value, quarter: "年报" },
  });
};

</script>
<style lang="scss" scoped>
.icon {
  width: 220px;
  height: 150px;
  margin-top: 10%;
  img {
    width: 100%;
    height: 100%;
  }
}
.page {
  width: 100%;
  height: calc(100% - 40px);
  display: flex;
  flex-direction: column;
  align-items: center;
}
main {
  width: 50%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.selectcss {
  width: 20%;
  margin-right: 2%;
}
.selectcss2 {
  width: 40%;
  margin-right: 2%;
}
@media screen and (max-width: 960px) {
  main {
    width: 70%;
  }
  .selectcss {
    width: 40%;
  }
}

@media screen and (max-width: 550px) {
  main {
    width: 80%;
  }
  .selectcss {
    width: 30%;
  }
}

.search {
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding-top: 10px;
  margin-bottom: 10%;
}
.hot {
  height: 10%;
  // width: 50%;
  h3 {
    height: 40px;
    line-height: 40px;
  }
  span {
    display: inline-block;
    padding: 5px 10px;
    cursor: pointer;
    color: #409eff;
    text-decoration: underline;
  }
}
.footer {
  width: 100%;
  height: 52px;
  background-color: #abb4bb;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  bottom: 0;
  left: 0;
  color: #fff;
  font-size: 12px;
  cursor: pointer;
  .call {
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
    text-decoration: none;
    .img {
      width: 16px;
      height: 16px;
      img {
        width: 100%;
        height: 100%;
        margin-left: 5px;
      }
    }
  }
  a {
    margin-right: 10px;
  }
}
</style>