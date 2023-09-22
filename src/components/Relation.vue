<template>
  <div>
    <div>【数据传递关系图】</div>
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
      xAxis:{
        show:false,
        type:'value'
      },
      yAxis:{
        show:false,
        type:'value'
      },
      series:[
        {
          type: 'graph',
          layout:'none',
          edgeSymbol: ['none', 'arrow'],
          edgeSymbolSize: 8,
          edgeLabel: {
            normal: {
              show: true,
              color: '#fff',
              textStyle: {
                fontSize: 14
              },
              formatter: function (params) {
                let txt = ''
                if (params.data.speed !== undefined) {
                  txt = params.data.speed
                }
                return txt
              }
            }
          },
          z:3,
          data:props.data.relations.map(item => {
            if(item.id === 0){
              return {
                name:item.name,
                value:item.value,
                symbolSize:100,
                itemStyle: {
                  normal: {
                    // 渐变色
                    color: {
                      colorStops: [
                        { offset: 0, color: '#157eff' },
                        { offset: 1, color: '#35c2ff' }
                      ]
                    }
                  }
							  },
                label:{
                  fontSize:14
                }
              }
            }else{
              return {
                name:item.name,
                value:item.value,
                active:true,
                speed: `${item.speed}kb/s`,
                category: 0,
              }
            }
          }),
          coordinateSystem :'cartesian2d',
          symbol:'circle',
          symbolSize:26,
          label:{
            show:true,
            position:'bottom',
            color:'#9EB1C8'
          },
          links:props.data.relations.map(item => (
              {
                source:item.source,
                target:item.target,
                speed: `${item.speed}kb/s`,
                lineStyle: { normal: { color: '#12b5d0', curveness: 0.2 } },
                label:{
                  show:true,
                  position:'middle',
                  offset:[10,0]
                }
              }
            )), 
        },
        {
          type:"lines",
          coordinateSystem :'cartesian2d',
          effect:{
            show:true,
            symbol:'arrow',
            symbolSize:10,
            color: 'rgba(55,155,255,0.5)',
            smooth: false,
          },
          lineStyle:{
            // color:'#fff',
            curveness:0.2
          },
          z:1,
          data: [
					[{ coord: [0, 300] }, { coord: [50, 200] }],
					[{ coord: [0, 100] }, { coord: [50, 200] }],
					[{ coord: [50, 200] }, { coord: [100, 100] }],
					[{ coord: [50, 200] }, { coord: [100, 300] }]
				],
        label:{
          show:true
        }
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