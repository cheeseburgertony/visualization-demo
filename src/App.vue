<script setup>
import { onMounted, ref } from 'vue'
import HorizontalBar from './components/HorizontalBar.vue'
import MapChart from './components/MapChart.vue'
import RadarBar from './components/RadarBar.vue'
import Relation from './components/Relation.vue'
import RingBar from './components/RingBar.vue'
import TotalData from './components/TotalData.vue'
import VerticalBar from './components/VerticalBar.vue'
import WordCloud from './components/WordCloud.vue'
import { getVisualizationAPI } from './apis/visualization'

const visualizationData = ref()
const getData = async () => {
  const res = await getVisualizationAPI()
  // console.log(res.data)
  visualizationData.value = res.data
}
getData()


setInterval(() => {
  getData()
}, 3000)
</script>

<template>
  <div class="bg-[url('assets/imgs/bg.jpg')] bg-cover bg-center h-screen text-white p-5 flex overflow-hidden"
    v-if="visualizationData">
    <!-- 左侧部分 -->
    <div class="flex-1 mr-5 bg-opacity-50 bg-slate-800 p-3 flex flex-col">
      <!-- 横向柱状图 -->
      <HorizontalBar class="h-1/3 box-border pb-4" :visualData="visualizationData.regionData"></HorizontalBar>
      <!-- 雷达图 -->
      <RadarBar class="h-1/3 box-border pb-4" :visualData="visualizationData.riskData"></RadarBar>
      <!-- 数据关系图 -->
      <Relation class="h-1/3" :visualData="visualizationData.relationData"></Relation>
    </div>
    <!-- 中间部分 -->
    <div class="w-1/2 mr-5 flex flex-col">
      <!-- 数据总览图 -->
      <TotalData class="bg-opacity-50 bg-slate-800 p-3" :visualData="visualizationData.totalData"></TotalData>
      <!-- 地图可视化 -->
      <MapChart class="bg-opacity-50 bg-slate-800 p-3 mt-4 flex-1" :visualData="visualizationData.mapData"></MapChart>
    </div>
    <!-- 右侧部分 -->
    <div class="flex-1 bg-opacity-50 bg-slate-800 p-3 flex flex-col">
      <!-- 竖向柱状图 -->
      <VerticalBar class="h-1/3 box-border pb-4" :visualData="visualizationData.serverData"></VerticalBar>
      <!-- 环形图（饼图） -->
      <RingBar class="h-1/3 box-border pb-4" :visualData="visualizationData.abnormalData"></RingBar>
      <!-- 文档词云图 -->
      <WordCloud class="h-1/3" :visualData="visualizationData.wordCloudData"></WordCloud>
    </div>
  </div>

</template>

<style scoped></style>
