<script setup>
import { CountUp } from 'countup.js';
import { onMounted, ref } from 'vue';

const props = defineProps({
  visualData: {
    type: Object,
    required: true
  }
})

console.log(props.visualData)
const totalCountTarget = ref()
const city1 = ref()
const city2 = ref()
const city3 = ref()
const city4 = ref()
const city5 = ref()
const city6 = ref()
/**
 * {
    "total": "6794739",
    "hb": "87788",
    "db": "65446",
    "hd": "76456",
    "zn": "42135",
    "xn": "74564",
    "xb": "75753"
}
 */


onMounted(() => {
  new CountUp(totalCountTarget.value, props.visualData.total).start()
  new CountUp(city1.value, props.visualData.hb).start()
  new CountUp(city2.value, props.visualData.db).start()
  new CountUp(city3.value, props.visualData.hd).start()
  new CountUp(city4.value, props.visualData.zn).start()
  new CountUp(city5.value, props.visualData.xn).start()
  new CountUp(city6.value, props.visualData.xb).start()
})


</script>

<template>
  <div class="p-6">
    <div class="text-slate-300 text-center">
      数据总量：
      <span ref="totalCountTarget" class="text-7xl ml-2 mr-2 font-bold font-[Electronic] text-gradient">
        679,473,929
      </span>
      条记录
    </div>
    <div class="mt-3 flex flex-wrap">
      <div class="w-1/3 text-center text-slate-400 text-sm">
        华北：
        <span ref="city1" class="text-[#5DC5EF] text-3xl font-[Electronic]">
          8,778,988
        </span>
      </div>
      <div class="w-1/3 text-center text-slate-400 text-sm">
        东北：<span ref="city2" class="text-[#5DC5EF] text-3xl font-[Electronic]">8,778,988</span>
      </div>
      <div class="w-1/3 text-center text-slate-400 text-sm">
        华东：<span ref="city3" class="text-[#5DC5EF] text-3xl font-[Electronic]">8,778,988</span>
      </div>
      <div class="w-1/3 text-center text-slate-400 text-sm">
        中南：<span ref="city4" class="text-[#5DC5EF] text-3xl font-[Electronic]">8,778,988</span>
      </div>
      <div class="w-1/3 text-center text-slate-400 text-sm">
        西南：<span ref="city5" class="text-[#5DC5EF] text-3xl font-[Electronic]">8,778,988</span>
      </div>
      <div class="w-1/3 text-center text-slate-400 text-sm">
        西北：<span ref="city6" class="text-[#5DC5EF] text-3xl font-[Electronic]">8,778,988</span>
      </div>
    </div>
  </div>
</template>