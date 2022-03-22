<template>
  <div class="charts" ref="charts">

  </div>
</template>

<script>
  import * as echarts from 'echarts';
    export default {
        name: "index",
        mounted(){
          //初始化echarts实例
          let lineCharts = echarts.init(this.$refs.charts)
          //配置数据
          lineCharts.setOption({
            xAxis:{
              //隐藏x轴
              show:false,
              type:'category'
            },
            yAxis:{
              show:false
            },
            //系列
            series:[
              {
                type:'line',
                smooth: true,
                data:[10,22,55,3,77,90],
                //拐点的样式
                itemStyle:{
                  //透明度
                  opacity:0
                },
                //线条颜色
                lineStyle:{
                  color:'purple',
                },
                //填充样式
                areaStyle:{
                  color: {
                    type: "linear",
                    x: 0,
                    y: 0,
                    x2: 0,
                    y2: 1,
                    colorStops: [
                      {
                        offset: 0,
                        color: "purple", // 0% 处的颜色
                      },
                      {
                        offset: 1,
                        color: "#fff", // 100% 处的颜色
                      },
                    ],
                    global: false, // 缺省为 false
                  }
                }
              }
            ],
            //布局调试
            grid:{
              left:0,
              right:0,
              top:0,
              bottom:0
            }
          })
      }
    }
</script>

<style scoped>
.charts{
  width: 100%;
  height: 100%;
}
</style>
