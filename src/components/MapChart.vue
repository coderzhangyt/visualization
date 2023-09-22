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
        //时间轴
      timeline:{
        // 数据
        data: props.data.voltageLevel,
        // 类目轴
        axisType: 'category',
        // 自动切换
        autoPlay: true,
        // 间隔时间
        playInterval: 3000,
        // 位置
        left: '10%',
        right: '10%',
        bottom: '0%',
        width: '80%',
        // 轴的文本标签
        label: {
          // 默认状态
          normal: {
            textStyle: {
              color: '#ddd'
            }
          },
          // 高亮状态
          emphasis: {
            textStyle: {
              color: '#fff'
            }
          }
        },
        // 文字大小
        symbolSize: 10,
        // 线的样式
        lineStyle: {
          color: '#555'
        },
        // 选中点的样式
        checkpointStyle: {
          borderColor: '#888',
          borderWidth: 2
        },
        // 控件样式
        controlStyle: {
          // 上一步按钮
          showNextBtn: true,
          // 下一步按钮
          showPrevBtn: true,
          // 默认样式
          normal: {
            color: '#666',
            borderColor: '#666'
          },
          // 高亮样式
          emphasis: {
            color: '#aaa',
            borderColor: '#aaa'
          }
        }
      },
      //柱形图
      baseOption:{
        grid:{

        },
      },
      //timeline 时间轴下的多个图表
      options:[]

    }


    props.data.voltageLevel.forEach((item,index) => {
      options.options.push()
    })

    myChart.setOption(options)
  }

  
  watch(() => props.data,()=>{
    renderChart()
  })
</script>

<style>

</style>