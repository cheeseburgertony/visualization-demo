<script setup>
import { onMounted, ref, watch } from 'vue';
import * as echarts from 'echarts'
import "echarts-wordcloud"

const props = defineProps({
  visualData: {
    type: Object,
    required: true
  }
})

// abnormals
// console.log(props.visualData.abnormals)

const target = ref()
let myCharts
onMounted(() => {
  myCharts = echarts.init(target.value)
  renderChart()
})

const randomRGB = () => {
  const r = parseInt(Math.random() * 255)
  const g = parseInt(Math.random() * 255)
  const b = parseInt(Math.random() * 255)
  return `rgb(${r}, ${g}, ${b})`
}


const renderChart = () => {
  const option = {
    series: [
      {
        type: 'wordCloud',
        sizeRange: [8, 46],
        rotationRange: [0, 0],
        gridSize: 0,
        layoutAnimation: true,
        textStyle: {
          color: randomRGB
        },
        emphasis: {
          textStyle: {
            fontWeight: 'bold',
            color: '#000'
          }
        },
        data: props.visualData.datas
      }
    ]
  }

  myCharts.setOption(option)
}

// 监视数据变化
watch(() => props.visualData, renderChart)

</script>

<template>
  <div>
    <div>【关键词条】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>