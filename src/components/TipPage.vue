<template>
  <el-dialog v-model="props.showTip" :show-close="false" width="300px">
    <div class="container">
      <p>
        微信扫码关注公众号：<b>看图读财报</b><br />
        发送：<b>shiniugu</b><br />
        获取解锁本站所有财报分析图表的验证码
      </p>
      <img src="../assets/ma.jpg" alt="" />
      <div class="submit">
        <el-input placeholder="请输入验证码" v-model="inputV"></el-input>
        <el-button type="primary" @click="submit">提交</el-button>
      </div>
    </div>
  </el-dialog>
</template>

<script setup>
import { ref, onMounted, computed, defineProps, watch, defineEmits } from "vue";
const props = defineProps({
  showTip: false,
});
const emits = defineEmits("updateVisible");
const inputV = ref();
const submit = () => {
  if (inputV.value === "shiniugu666") {
    emits("updateVisible", false);
    localStorage.setItem("deal", "看过啦");
  } else {
    console.log("111");
    ElMessage({
      message: "验证码输入错误",
      type: "error",
    });
  }
  inputV.value = "";
};
</script>

<style lang="scss" scoped>
.submit {
  display: flex;
  padding: 0 10%;
  .el-button {
    margin-left: 10px;
  }
}
</style>