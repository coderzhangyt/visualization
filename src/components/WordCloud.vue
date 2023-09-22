<template>
  <div>
    <div>【关键词条】</div>
    <div ref="target" class=" w-full h-full"></div>
  </div>
</template>

<script setup>
import {onMounted, ref, watch} from 'vue'
import * as echarts from 'echarts';
import 'echarts-wordcloud'

  const props = defineProps({
    data:{
      type:Object,
      required:true
    }
  })
  console.log(props.data);

  const target = ref(null)
  let myChart = null
  onMounted(()=>{
    //初始化
    myChart = echarts.init(target.value)
    renderChart()
  })
 
  //指定图表的配置项和数据
  const renderChart = () => {
    const randomRGB = () => {
      const r = Math.floor(Math.random() * 255)
      const g = Math.floor(Math.random() * 255)
      const b = Math.floor(Math.random() * 255)
      return `rgb(${r},${g},${b})`
    }
    const options = {
      series:[
        {
          type:'wordCloud',
          sizeRange:[8,46],//字体大小范围
          rotationRange:[0,0],//旋转
          gridSize:0,
          layoutAnimation:true,//动画
          textStyle:{
            //字体颜色
            color:randomRGB
          },
          emphasis:{
            textStyle:{
              fontWeight:'bold',
              color:'#000'
            }
          },
          data:props.data.datas
        }
      ]
    }
    myChart.setOption(options)
  }

  watch(() => props.data,()=>{
    renderChart()
  })
  
</script>

<style>

</style>