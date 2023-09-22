<template>
  <div>
    <div>【大区异常处理】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
	data: {
		type: Object,
		required: true
	}
})

const target = ref(null)
let mChart = null
onMounted(() => {
	mChart = echarts.init(target.value)
	renderChart()
})

/**
 * 双环形图绘制原理：
 * 1. 环形图通过饼图绘制。内外边距的距离减小，即为环形。环形中心点需要不断改变，否则会重叠
 * 2. 环形图绘制分为 上层和底层 两部分。上层作为绘制进度，底层作为背景图
 * 3. 依据 getSeriesData 生成对应的 上层和底层 series 数据，进行渲染
 */
const getSeriesData = () => {
	const series = []

	props.data.abnormals.forEach((item, index) => {
		// 上层环形绘制
		series.push({
			name:item.name,
			type:"pie",
			data:[
				{
					value:item.value,
					name:item.name
				},
				{
					value:1000,
					itemStyle: { color: 'rgba(0,0,0,0)', borderWidth: 0 },
					tooltip:{
						show:false
					}
				}
			],
			center:['55%','55%'],
			radius:[73-index*15+'%',68-index*15+'%'],
			hoverAnimation:false,
			clockWise:false,
			label:{
				show:false
			},
			itemStyle:{
				borderWidth:0,
				borderRadius:5
			}

		})

		// 底层图
		series.push({
			name: item.name,
			type: 'pie',
			// 图形不响应事件
			silent: true,
			// z-index: 置于底层
			z: 1,
			// 逆时针排布
			clockWise: false,
			// 不展示鼠标移入动画
			hoverAnimation: false,
			// 半径位置，需要依次递减，否则会重复在一处进行展示
			radius: [73 - index * 15 + '%', 68 - index * 15 + '%'],
			// 中心点
			center: ['55%', '55%'],
			// 不展示 label
			label: { show: false },
			// 数据
			data: [
				// 绘制底线 75%
				{
					value: 7.5,
					itemStyle: { color: 'rgb(3, 31, 62)', borderWidth: 0 },
					tooltip: { show: false },
					hoverAnimation: false
				},
				// 绘制底线 25% 透明区域
				{
					value: 2.5,
					name: '',
					itemStyle: { color: 'rgba(0,0,0,0)', borderWidth: 0 },
					tooltip: { show: false },
					hoverAnimation: false,
				}
			]
		})
	})

	return series
}

const renderChart = () => {
	const options = {
		//图例
		legend:{
			show:true,
			top:'14%',
			left:"60%",
			icon:'circle',
			data: props.data.abnormals.map((item) => item.name),
			width:-5,
			itemWidth :10,
			itemHeight:10,
			itemGap:6,
			textStyle:{
				color:'#9EB1C8',
				lineHeight:5,
				fontSize:10
			}
		},
		//提示层
		tooltip:{
			show:true,
			trigger:'item',
			formatter: '{a}<br>{b}:{c}({d}%)'
		},
		
		// 每两个标记一条线
		series: getSeriesData()
	}

	mChart.setOption(options)
}

// 监听数据的变化，重新渲染图表
watch(
	() => props.data,
	() => {
		renderChart()
	}
)
</script>

<style>

</style>