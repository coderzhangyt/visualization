<script setup>
import HorizontalBar from './components/HorizontalBar.vue';
import MapChart from './components/MapChart.vue';
import RadarBar from './components/RadarBar.vue';
import Relation from './components/Relation.vue';
import RingBar from './components/RingBar.vue';
import TotalData from './components/TotalData.vue';
import VerticalBar from './components/VerticalBar.vue';
import WordCloud from './components/WordCloud.vue';

import { getVisualization } from './api/visualization';
import {onMounted, reactive, ref} from 'vue'

const chartData = ref(null)
//发送请求
const loadData = async()=>{
  chartData.value = await getVisualization()
  chartData.value = chartData.value.data.data
  // console.log(chartData.value.regionData);
}
loadData()

//数据在不停的变化，设置定时器，隔段时间获取到不一样的数据
setInterval(()=>{
  loadData()
},3000)

</script>

<template>
  <div 
  class="bg-[url('assets/imgs/bg.jpg')] bg-cover bg-center h-screen text-white p-5 flex overflow-hidden"
  v-if="chartData"
  >
    <!-- 左 -->
    <div class="flex-1 mr-5 bg-opacity-50 bg-slate-800 p-3 flex flex-col">

      <!-- 横向柱状图 -->
      <HorizontalBar class=" h-1/3 box-border pb-4" :data="chartData.regionData" />
      <!-- 雷达图 -->
      <RadarBar class=" h-1/3 box-border pb-4" :data="chartData.riskData" />
      <!-- 关系图 -->
      <Relation class=" h-1/3" :data="chartData.relationData" />

    </div>
    <!-- 中 -->
    <div class="w-1/2 mr-5 flex flex-col">

      <!-- 数据总览图 -->
      <TotalData class=" bg-opacity-50 bg-slate-800 p-3" :data="chartData.totalData" />
      <!-- 地图可视化 -->
      <MapChart  class=" bg-opacity-50 bg-slate-800 flex-1 p-3 mt-4" :data="chartData.mapData" />

    </div>
    <!-- 右 -->
    <div class="flex-1 bg-opacity-50 bg-slate-800 p-3 flex flex-col">


      <!-- 纵向柱状图 -->
      <VerticalBar class=" h-1/3 box-border pb-4" :data="chartData.serverData" />
      <!-- 环形图 -->
      <RingBar class=" h-1/3 box-border pb-4" :data="chartData.abnormalData" />
      <!-- 文档云图 -->
      <WordCloud class=" h-1/3"  :data="chartData.wordCloudData" />

    </div>
  </div>
</template>

<style scoped>

</style>
