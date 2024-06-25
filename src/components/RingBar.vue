<script setup>
import { TrackOpTypes, onMounted, ref, watch } from 'vue';
import * as echarts from 'echarts'

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

const getSeriesData = () => {
  const series = []

  props.visualData.abnormals.forEach((item, index) => {
    // 饼图上层
    series.push({
      name: item.name,
      type: 'pie',
      clockwise: false,
      emphasis: {
        scale: false // 鼠标移入显示动画
      },
      radius: [73 - index * 15 + '%', 68 - index * 15 + '%'],
      center: ['55%', '55%'],
      label: {
        show: false
      },
      data: [
        {
          value: item.value,
          name: item.name
        },
        {
          // 配置最大数据
          type: ' hideTip',
          name: item.name,
          value: 1000,
          itemStyle: {
            color: 'rgba(0,0,0,0)',
            borderWidth: 1
          },
          tooltip: {
            show: false
          },
          emphasis: {
            scale: false // 鼠标移入显示动画
          },
        }
      ]
    })

    // 饼图底层
    series.push({
      name: item.name,
      type: 'pie',
      silent: true,  // 不受其他事件影响
      z: 1,
      clockwise: false,
      emphasis: {
        scale: false // 鼠标移入显示动画
      },
      radius: [73 - index * 15 + '%', 68 - index * 15 + '%'],
      center: ['55%', '55%'],
      label: {
        show: false
      },
      data: [
        {
          value: 7.5,
          itemStyle: {
            color: 'rgb(3,31,62)',
            borderWidth: 0
          },
          tooltip: {
            show: false,
          },
          emphasis: {
            scale: false // 鼠标移入显示动画
          }
        },
        {
          value: 2.5,
          itemStyle: {
            color: 'rgba(0,0,0,0)',
            borderWidth: 0
          },
          tooltip: {
            show: false,
          },
          emphasis: {
            scale: false
          }
        }
      ]
    })
  })

  return series
}

const renderChart = () => {
  const option = {
    // 图例配置
    legend: {
      show: true,
      icon: 'circle',
      top: '14%',
      left: '60%',
      data: props.visualData.abnormals.map(item => item.name),
      width: -5, // 指定宽度让其一个图例占一行
      itemWidth: 10,
      itemHeight: 10,
      itemGap: 6, // 图例间的间距
      textStyle: {
        fontSize: 12,
        lineHeight: 5,
        color: '#ffffff'
      }
    },
    // 提示层
    tooltip: {
      show: true,
      trigger: 'item', // 触发方式 item 表示数据项触发
      formatter: '{a}<br>{b}:{c}({d}%)'
    },
    // Y轴
    yAxis: [
      {
        show: false,
        type: 'category',
        inverse: true
      }
    ],
    // X轴
    xAxis: [
      {
        show: false
      }
    ],
    // 核心配置
    series: getSeriesData()
  }

  myCharts.setOption(option)
}

// 监视数据变化
watch(() => props.visualData, renderChart)

</script>

<template>
  <div>
    <div>【大区异常处理】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>