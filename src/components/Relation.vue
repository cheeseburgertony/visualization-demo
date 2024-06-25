<script setup>
import { effect, onMounted, ref, watch } from 'vue';
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
      show: false,
      type: 'value'
    },
    yAxis: {
      show: false,
      type: 'value'
    },
    series: [
      {
        type: 'graph',
        layout: 'none',
        coordinateSystem: 'cartesian2d', // 使用二维的直角坐标系
        symbolSize: 26,  // 节点大小
        z: 3,
        edgeLabel: {
          show: true,
          color: '#fff',
          fontSize: 14,
          formatter: (params) => params.data.speed
        },
        label: {
          show: true,
          position: 'bottom',
          color: '#5E5E5E'
        },
        edgeSymbol: ['none', 'arrow'], // 箭头
        edgeSymbolSize: 8,
        data: props.visualData.relations.map(item => {
          if (item.id !== 0) {
            // 非数据中心
            return {
              name: item.name,
              category: 0,
              active: true,
              speed: `${item.speed}kb/s`,
              value: item.value
            }
          } else {
            // 数据中心
            return {
              name: item.name,
              value: item.value,
              symbolSize: 100,
              itemStyle: {
                color: {  // 渐变色
                  colorStops: [
                    {
                      offset: 0,
                      color: '#157eff'
                    },
                    {
                      offset: 1,
                      color: '#35c2ff'
                    }
                  ]
                }
              },
              label: {
                fontSize: '14'
              }
            }
          }
        }),
        // 连接线
        links: props.visualData.relations.map((item, index) => ({
          source: item.source,
          target: item.target,
          speed: `${item.speed}kb/s`,
          lineStyle: {
            color: '#12b5d0',
            curveness: 0.2
          },
          label: {
            show: true,
            position: 'middle',
            offset: [10, 0]  // 进行偏移，防止字被数据中心档到
          }
        }))
      },
      {
        // 用于带有起点和终点信息的线数据的绘制，主要用于地图上的航线，路线的可视化
        type: 'lines',
        coordinateSystem: 'cartesian2d', // 使用二维的直角坐标系
        z: 1,
        effect: { // 线特效的配置
          show: true,
          smooth: false,
          trailLength: 0,
          symbol: 'arrow',
          color: 'rgba(55,155,255,0.6)',
          symbolSize: 12
        },
        lineStyle: {
          curveness: 0.2
        },
        data: [ // 线数据集
          [{ coord: [0, 300] }, { coord: [50, 200] }],  //  [起点, 终点]
          [{ coord: [0, 100] }, { coord: [50, 200] }],
          [{ coord: [50, 200] }, { coord: [100, 100] }],
          [{ coord: [50, 200] }, { coord: [100, 300] }]
        ]
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
    <div>【数据传递信息】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>