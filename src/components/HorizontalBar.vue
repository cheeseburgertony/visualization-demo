<script setup>
import * as echarts from 'echarts'
import { ref, onMounted, watch } from 'vue';
const props = defineProps({
  visualData: {
    type: Object,
    required: true
  }
})
console.log(props.visualData)

// 1.初始化echarts实例
const target = ref()

let myCharts
onMounted(() => {
  myCharts = echarts.init(target.value)
  renderChart()
})

// 2.构建option对象
const renderChart = () => {
  const option = {
    // X轴展示数据
    // show 是否显示 x 轴
    // type 坐标轴类型  'value' 数值轴，适用于连续数据 这里x轴是数据
    // max 坐标轴刻度最大值 value 是一个包含 min 和 max 的对象 这里*1.2为了防止触顶，因为还有数字
    xAxis: {
      show: false,
      type: 'value',
      max: (value) => parseInt(value.max * 1.2)
    },
    // Y轴展示数据
    // type  'category' 类目轴，适用于离散的类目数据。 这里用做属性
    // data 类目数据，在类目轴（type: 'category'）中有效 返回一个数组 这里返回一个由名字构成的数组
    // inverse 是否是反向坐标轴
    // axisLine 坐标轴轴线相关设置
    // axisTick 坐标轴刻度相关设置  这里设置不展示轴线和刻度但是展示文字
    // axisLabel 坐标轴刻度标签的相关设置
    yAxis: {
      type: 'category',
      data: props.visualData.regions.map(item => item.name),
      inverse: true,
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        color: '#9eb1c8'
      }
    },
    // 图表绘制的位置，对应上下左右
    // 已经设置了padding了所以全部置为0就可以
    // containLabel 表示 grid 区域是否包含坐标轴的刻度标签  计算时把标签包含进去
    grid: {
      top: 0,
      bottom: 0,
      left: 0,
      right: 0,
      containLabel: true
    },
    // 核心配置
    // type: 'bar' 表示柱状图
    // data 数组包对象，对象包括名字和数据
    // showBackground 是否显示柱条的背景色。
    // backgroundStyle 每一个柱条的背景样式
    // itemStyle 图形样式
    // barWidth 每个柱子宽
    // label 柱子后面的数字
    series: [{
      type: 'bar',
      data: props.visualData.regions.map(item => ({
        name: item.name,
        value: item.value
      })),
      showBackground: true,
      backgroundStyle: {
        color: 'rgba(180, 180, 180, 0.2)'
      },
      itemStyle: {
        color: '#479AD3',
        borderRadius: 5,  // 圆角
        shadowColor: 'egba(0,0,0,0.3)',
        shadowBlur: 5   // 阴影的模糊
      },
      barWidth: 12,
      label: {
        show: true,
        position: 'right',
        color: '#fff'
      }
    }]
  }
  // 3.通过实例.setOption(option) 生成图表
  myCharts.setOption(option)
}


// 监听数据的变化改变视图
// 如果需要监视对象中的某一个属性的时候，且该属性是基本类型的，就要写成函数式
watch(() => props.visualData, () => {
  renderChart()
})

</script>

<template>
  <div>
    <div>【大区数据信息】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>