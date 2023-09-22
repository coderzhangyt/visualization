<template>
  <div>
    <div>【服务资源占用比】</div>
    <div ref="target"  class=" w-full h-full"></div>
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
    const options = {
      yAxis: { 
        type:'value',
        show:false,
        max:function(value){
          return value.max * 1.2
        }
      },
      xAxis: { 
        type: 'category',
        data: props.data.servers.map(item => item.name),
        axisLabel:{
          color:'#9EB1C8'
        },
      },
      // echarts 网格绘制的位置，对应 上、右、下、左
      grid:{
        left:0,
        right:0,
        top:'7%',
        bottom:'15%',
      },
      series:[
      {
        type:'bar',
        data:props.data.servers.map(item=>({
          name:item.name,
          value:item.value
        })),
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
          position:'top',
          formatter:'{c}%'
        }
      }
      ],
    }
    myChart.setOption(options)
  }

  watch(() => props.data,()=>{
    renderChart()
  })
</script>

<style>

</style>