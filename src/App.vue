<template>
  <h1>{{ title }}</h1>
  <div>
    <div v-for="(d, ind) in data" :key="ind" style="display: flex">
      <h3>{{ d.text }}</h3>
      <input type="text" v-model="data[ind]['text']" />
      <button @click="updateData(data[ind]['id'], data[ind]['text'])">
        수정
      </button>
      <button @click="deleteData(data[ind]['id'])">삭제</button>
    </div>
  </div>
  <div
    class="loading"
    v-if="loading"
    style="
      position: fix;
      width: 100vw;
      height: 100vh;
      background-color: rgba(0, 0, 0, 0.5);
    "
  ></div>
  <router-view style="display: none"></router-view>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
const loading = ref(false);

const title = ref("vue3-wonjin-template");
const url =
  "https://script.google.com/macros/s/AKfycbxIP5cFhM0AW9Vv-bQjUet76rGT0n4c9W1gp0Vt9VzR9LRCdr3x7ywWqz51t98plbiJ7w/exec";

const data = ref([]);

async function getData() {
  loading.value = true;
  const dataSheet = await axios.get(url);
  console.log(dataSheet.data.data.obj);

  data.value = dataSheet.data.data.obj;
  loading.value = false;
}

// async function postData() {
//   const dataSheet = await axios.get(url);
// }

async function updateData(id, data) {
  loading.value = true;
  const res = await axios.post(
    url + "?tasktype=update",
    JSON.stringify({ id, data })
  );

  await getData();
  loading.value = false;
}

async function deleteData(id) {
  loading.value = true;
  const res = await axios.post(
    url + "?tasktype=delete",
    JSON.stringify({ id })
  );

  await getData();
  loading.value = false;
}

getData();
</script>

<style lang="scss" scoped>
h1 {
  text-align: center;
  color: $my-color;
  font-size: $my-size;
}
</style>
