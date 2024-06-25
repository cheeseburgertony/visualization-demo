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
    xAxis: {
      type: 'category',
      data: props.visualData.servers.map(item => item.name),
      axisLabel: {
        color: '#9EB1C8'
      }
    },
    yAxis: {
      type: 'value',
      show: false,
      max: (value) => parseInt(value.max * 1.2)
    },
    grid: {
      top: 16,
      right: 0,
      bottom: 26,
      left: -26,
      containLabel: true
    },
    series: [{
      type: 'bar',
      data: props.visualData.servers.map(item => ({
        name: item.name,
        value: item.value
      })),
      itemStyle: {
        color: '#479AD3',
        // borderRadius: 5,  // 圆角
        shadowColor: 'egba(0,0,0,0.3)',
        shadowBlur: 5   // 阴影的模糊
      },
      barWidth: 12,
      label: {
        show: true,
        position: 'top',
        color: '#fff',
        formatter: '{c}%'  // 修改显示样式 {c}是数据 花括号里一定要是c表示数据 在花括号外面可以加别的修饰
      }
    }
    ]
  }
  myCharts.setOption(option)
}

// 监视数据发生变化
watch(() => props.visualData, renderChart)
/* 
// 这里的写法等同以下的写法
watch(() => props.visualData, () => {
  renderChart
}) */


</script>

<template>
  <div>
    <div>【服务资源占比】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>