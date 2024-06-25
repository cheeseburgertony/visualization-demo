<script setup>
import { onMounted, ref, watch } from 'vue';
import * as echarts from 'echarts'
import mapJson from '@/assets/MapData/china.json'

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
  echarts.registerMap('china', mapJson)
  myCharts = echarts.init(target.value)
  renderChart()
})

const renderChart = () => {
  const option = {
    // 时间线
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
    },
    // 柱形图
    baseOption: {
      grid: {
        right: '2%',
        top: '15%',
        bottom: '10%',
        width: '20%',
      },
      // 地图
      geo: {
        show: true,
        map: 'china',
        roam: true,
        zoom: 0.8,
        center: [113.83531246, 34.0267395887],
        itemStyle: {
          borderColor: 'rgba(147, 235, 248, 1)',
          borderWidth: 1,
          areaColor: {
            type: 'radial',
            x: 0.5,
            y: 0.5,
            r: 0.5,
            colorStops: [
              {
                offset: 0,
                color: 'rgba(147, 235, 248, 0)'
              },
              {
                offset: 1,
                color: 'rgba(147, 235, 248, 0.2)'
              }
            ]
          },
        },
        emphasis: {
          itemStyle: {
            areaColor: '#389BB7',
            borderWidth: 0
          }
        }
      }
    },
    options: []
  }

  props.visualData.voltageLevel.forEach((item, index) => {
    option.options.push({
      backgroundColor: '#142037',
      title: [
        {  // 大标题
          text: '2019-2023年度数据',
          left: '0%',
          top: '0%',
          textStyle: {
            color: '#ccc',
            fontSize: 30
          }
        },
        {
          // 小标题
          id: 'statistic',
          text: item + '年数据统计情况',
          right: '0%',
          top: '4%',
          textStyle: {
            color: '#ccc',
            fontSize: 20
          }
        }
      ],
      xAxis: {
        type: 'value',
        scale: true,
        position: 'top',
        splitLine: {
          show: false
        },
        axisLine: {
          show: false
        },
        axisTick: {
          show: false
        },
        axisLabel: {
          margin: 2,
          color: '#aaa'
        }
      },
      yAxis: {
        type: 'category',
        axisLine: {
          show: true,
          lineStyle: {
            color: '#ddd'
          }
        },
        axisTick: {
          show: false
        },
        axisLabel: {
          interval: 0,
          color: '#ddd'
        },
        data: props.visualData.categoryData[item].map(item => item.name)
      },
      series: [
        {
          type: 'bar',
          zlevel: 1.5,
          itemStyle: {
            color: props.visualData.colors[index]
          },
          data: props.visualData.categoryData[item].map(item => item.value)
        },
        {
          // 散点图
          type: 'effectScatter',
          coordinateSystem: 'geo',
          data: props.visualData.topData[item],
          symbolSize: (val) => val[2] / 4,  // 设置散点大小
          showEffectOn: 'render', // 添加特效
          rippleEffect: {
            brushType: 'stroke' // 水花效果
          },
          label: {
            formatter: '{b}',
            position: 'right',
            show: true
          },
          itemStyle: {
            // 保持和柱状图颜色一致
            color: props.visualData.colors[index],
            shadowBlur: 5,  // 阴影
            shadowColor: props.visualData.colors[index]
          }
        }
      ]
    })
  })

  myCharts.setOption(option)
}

// 监视数据发生变化
watch(() => props.visualData, renderChart)

</script>

<template>
  <div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>