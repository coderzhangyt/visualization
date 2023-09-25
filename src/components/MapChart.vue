<template>
  <div>
    <!-- <div>【服务资源占用比】</div> -->
    <div ref="target"  class=" w-full h-full"></div>
  </div>
</template>

<script setup>
import {onMounted, ref, watch} from 'vue'
import * as echarts from 'echarts';
import mapJson from '../assets/MapData/china.json'

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
    echarts.registerMap('china',mapJson)
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
        //柱形图的位置和大小
        grid:{
          right:'2%',
          top:'15%',
          width:'20%'
        },
       title:[
        {
            text:'2019-2023 年度数据统计',
            textStyle:{
              color:'#ccc',
              fontSize:30
            }
          },
       ],
       geo:{
        show:true,
        map:'china',
        roam:true,
        center:[113.83531246, 34.0267395887],
        itemStyle:{
          normal: {
						// 边框色值
						borderColor: 'rgba(147, 235, 248, 1)',
						// 边框宽度
						borderWidth: 1,
						// 区域颜色
						areaColor: {
							// 经向色值
							type: 'radial',
							x: 0.5,
							y: 0.5,
							r: 0.5,
							colorStops: [
								// 0% 处的颜色
								{
									offset: 0,
									color: 'rgba(147, 235, 248, 0)'
								},
								// 100% 处的颜色
								{
									offset: 1,
									color: 'rgba(147, 235, 248, .2)'
								}
							],
							// 缺省为 false
							globalCoord: false
						}
					},
					// 鼠标移入的色值
					emphasis: {
						areaColor: '#389BB7',
						borderWidth: 0
					}
        }
       }
      },
      //timeline 时间轴下的多个图表
      options:[]

    }

    props.data.voltageLevel.forEach((item,index) => {
      // console.log(props.data.categoryData[item].map(i => i.name));
      options.options.push({
        backgroundColor: '#142037',
        title:[
          {
            id:'statistic',
            text:item + '年数据统计情况',
            right:'2%',
            top:'4%',
            textStyle:{
              color:'#ccc',
              fontSize:20
            }
          }
        ],
        xAxis:{
          type:'value',
          position:'top',
          axisTick:{
            show:false
          },
          splitLine:{
            show:false
          },
          splitNumber:10,
          // 脱离 0 值比例
				  // scale: true,
        },
        yAxis:{
          type:'category',
          show:true,
          data:props.data.categoryData[item].map(i => i.name),
          axisLine: {
            show: true,
            lineStyle: {
              color: '#ddd'
            }
          },
          axisTick:{
            show:false
          },
          axisLabel:{
            fontSize:7
          }
        },
        series:[{
          type:'bar',
          zlevel:1.5,
          data:props.data.categoryData[item].map(i => i.value),
          itemStyle:{
            color:props.data.colors[index],
            height:10
          }
        },
        {
          type:'effectScatter',
          coordinateSystem:'geo',
          data:props.data.topData[item],
          rippleEffect :{
            brushType:'stroke'
          },
          symbolSize:function (val) {
            return val[2]/4
          },
          label:{
            normal:{
              show:true,
              position:'right',
              formatter:'{b}'
            }
          },
          itemStyle:{
            normal:{
              color:props.data.colors[index]
            }
          }
        }]
      })
    })

    myChart.setOption(options)
  }

  
  watch(() => props.data,()=>{
    renderChart()
  })
</script>

<style>

</style>