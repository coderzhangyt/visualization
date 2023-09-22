<template>
  <div>
    <div>【大区数据信息】</div>
    <div ref="target" class=" w-full h-full"></div>
  </div>
</template>

<script setup>
import {onMounted, ref, watch} from 'vue'
import * as echarts from 'echarts';

  const props = defineProps({
    data:{
      type:Object,
      required:true
    }
  })
  // console.log(props.data);

  const target = ref(null)
  let myChart = null
  onMounted(()=>{
    //初始化
    myChart = echarts.init(target.value)
    renderChart()
  })

  //指定图表的配置项和数据
  const renderChart = () => {
    const options = {
      xAxis: { 
        type:'value',
        show:false ,
        max:function(value){
          return value.max * 1.2
        },
      },
      yAxis: { 
        type: 'category',
        data: props.data.regions.map(item => item.name),
       inverse:true,//反向展示
        axisLine:{
          show:false //取消轴线
        },
        axisTick:{
          show:false //不展示轴线刻度
        },
        axisLabel:{
          color:'#9EB1C8'
        }
      },
      // echarts 网格绘制的位置，对应 上、右、下、左
      grid:{
        left:0,
        right:0,
        top:0,
        bottom:0,
        //计算边距时，包含标签
        containLabel:true
      },
      series:{
        type:'bar',
        data:props.data.regions.map(item=>({
          name:item.name,
          value:item.value
        })),
        //轴的背景
        showBackground:true,
        backgroundStyle:{
          color:'rgba(180,180,180,0.2)'
        },
        //每个轴的样式
        itemStyle:{
          color:'rgb(94, 152, 207)',
          barBorderRadius:10,
          shadowColor:'rgba(0,0,0,0.3)',
          shadowBlur:5
        },
        //轴宽度
        barWidth:12,
        //轴上的字体
        label:{
          show:true,
          color:'#fff',
          position:'right'
        }
      }

    }
    myChart.setOption(options)
  }

  watch(() => props.data,()=>{
    renderChart()
  })
  

</script>

<style>

</style>