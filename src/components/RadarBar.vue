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
onMounted(() => {
  myCharts = echarts.init(target.value)
  renderChart()
})

const renderChart = () => {
  const option = {
    // 雷达图的坐标系配置
    radar: {
      axisName: {
        color: '#05D5FF',
        fontSize: 14
      },
      shape: 'polygon',
      center: ['50%', '50%'],
      radius: '80%',  // 半径
      startAngle: 120,  // 起始角度
      // 轴线
      axisLine: {
        show: true,
        lineStyle: {
          color: 'rgb(5, 213,255, 0.8)'
        },
      },
      // 网格线
      splitLine: {
        show: true,
        lineStyle: {
          width: 1,
          color: 'rgb(5, 213,255, 0.8)'
        },
      },
      // 指示器  展示图表中的名字
      indicator: props.visualData.risks.map(item => ({
        name: item.name,
        max: 100
      })),
      // 是否显示分隔区域
      splitArea: {
        show: false
      }
    },
    // 坐标极点
    polar: {
      center: ['50%', '50%'],
      radius: '0%'
    },
    // 坐标角度
    angleAxis: {
      min: 0,
      interval: 5, // 分割间隔
      closewise: false // 是否以顺时针增长
    },
    // 径向轴
    radiusAxis: {
      min: 0,
      interval: 20,
      splitLine: {
        show: false
      }
    },
    // 图表核心配置
    series: {
      type: 'radar',
      symbol: 'circle',
      symbolSize: 10,
      // 小圆点
      itemStyle: {
        color: '#05D5FF'
      },
      // 覆盖的区域
      areaStyle: {
        color: '#05D5FF',
        opacity: 0.5
      },
      // 线条
      lineStyle: {
        width: 2,
        color: '#05D5FF'
      },
      // 文字
      label: {
        show: true,
        color: '#fff'
      },
      data: [
        {
          value: props.visualData.risks.map(item => item.value)
        }
      ]
    }
  }

  myCharts.setOption(option)
}

// 监视数据变化
watch(() => props.visualData, renderChart)

</script>

<template>
  <div>
    <div>【云端报警风险】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>