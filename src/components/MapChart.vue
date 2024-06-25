<script setup>
import { onMounted, ref, watch } from 'vue';
import * as echarts from 'echarts'

const props = defineProps({
  visualData: {
    type: Object,
    required: true
  }
})

const target = ref()
let myCharts
// console.log(props.visualData)
onMounted(() => {
  myCharts = echarts.init(target.value)
  renderChart()
})

const renderChart = () => {
  const option = {
    timeline: {
      data: props.visualData.voltageLevel,
      axisType: 'category',
      autoPlay: true,
      playInterval: 2000,
      left: '10%',
      right: '10%',
      bottom: '0%',
      width: '80%',
      label: {
        color: '#ddd'
      },
      emphasis: {
        label: {
          color: '#fff'
        }
      },
      symbolSize: 10,
      lineStyle: {
        color: '#555'
      },
      checkpointStyle: {
        borderColor: '#888',
        corderWidth: 2
      },
      controlStyle: {
        showNextBtn: true,
        showPrevBtn: true,
        color: '#666',
        borderColor: '#666',
      },
      emphasis: {  // 高亮
        controlStyle: {
          color: '#aaa',
          borderColor: '#aaa'
        }
      }
    }
  }
  myCharts.setOption(option)
}

// 监视数据发生变化
watch(() => props.visualData, renderChart)

</script>

<template>
  <div>
    <div>【地图可视化】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>