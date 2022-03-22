<template>
  <el-card class="box-card" style="margin:10px 0 ">
    <!--头部区域-->
    <div slot="header" class="clearfix">
      <!-- @tab-click="handleClick"-->
      <el-tabs class="tab" v-model="activeName">
        <el-tab-pane label="销售额" name="sale"></el-tab-pane>
        <el-tab-pane label="访问量" name="visite"></el-tab-pane>
       </el-tabs>

      <!--头部右侧的内容-->
      <div class="right">
        <span @click="setDay">今日</span>
        <span @click="setWeek">本周</span>
        <span @click="setMonth">本月</span>
        <span @click="setYear">本年</span>
        <el-date-picker
          v-model="date"
          class="date"
          type="daterange"
          range-separator="-"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          size="mini"
          value-format="yyyy-MM-dd">
        </el-date-picker>
      </div>
    </div>

    <!--内容区域-->
    <div>
      <el-row :gutter="10">
        <el-col :span="18">
          <!--柱状图-->
          <div class="charts" ref="charts"></div>
        </el-col>

        <el-col :span="6">
         <h3>{{title}}排名</h3>
          <ul>
            <li>
              <span class="rindex">1</span>
              <span>肯德基</span>
              <span class="rvalue">123456</span>
            </li>
            <li>
              <span class="rindex">2</span>
              <span>肯德基</span>
              <span class="rvalue">123456</span>
            </li>
            <li>
              <span class="rindex">3</span>
              <span>肯德基</span>
              <span class="rvalue">123456</span>
            </li>
            <li>
              <span class="rindex">4</span>
              <span>肯德基</span>
              <span class="rvalue">123456</span>
            </li>
            <li>
              <span class="rindex">5</span>
              <span>肯德基</span>
              <span class="rvalue">123456</span>
            </li>
            <li>
              <span class="rindex">6</span>
              <span>肯德基</span>
              <span class="rvalue">123456</span>
            </li>
            <li>
              <span class="rindex">7</span>
              <span>肯德基</span>
              <span class="rvalue">123456</span>
            </li>
          </ul>
        </el-col>
      </el-row>
    </div>
  </el-card>
</template>

<script>
  import * as echarts from 'echarts';
  import dayjs from 'dayjs'
  import {mapState} from 'vuex'
    export default {
        name: "sale",
      data(){
          return{
            activeName:'sale',
            mycharts:null,
            //日期数据
            date:[]
          }
      },
      mounted(){
          //初始化echarts实例
         this.mycharts=echarts.init(this.$refs.charts);
         //配置数据
         this.mycharts.setOption({
           title:{
             text:this.title+'趋势'
           },
           tooltip: {
             trigger: 'axis',
             axisPointer: {
               type: 'shadow'
             }
           },
           grid: {
             left: '3%',
             right: '4%',
             bottom: '3%',
             containLabel: true
           },
           xAxis: [
             {
               type: 'category',
               data: [],
               axisTick: {
                 alignWithLabel: true
               }
             }
           ],
           yAxis: [
             {
               type: 'value'
             }
           ],
           series: [
             {
               name: 'Direct',
               type: 'bar',
               barWidth: '60%',
               data: [],
               color:'red'
             }
           ]
         })


      },
      computed:{
          //
          title(){
            return this.activeName == 'sale'?'销售额':'访问量'
          },
          ...mapState({
          listState:state =>state.home.list
        })
      },
      //监听属性
      watch:{
          title(){
            //重新修改图标的配置数据
            //图标配置数据可以再次修改，如果有新的数值，新的数值，没有新的数值，还是用以前的
            this.mycharts.setOption({
            title:{
              text:this.title+'趋势'
            },
              xAxis:{
              data:this.title == '销售额'?this.listState.orderFullYearAxis : this.listState.userFullYearAxis
              },
              series:{
                name: 'Direct',
                type: 'bar',
                barWidth: '60%',
                data: this.title == '销售额'?this.listState.orderFullYear : this.listState.userFullYear,
                color:'red'
              }
          })

        },
          listState(){
            this.mycharts.setOption({
              title:{
                text:this.title+'趋势'
              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '3%',
                containLabel: true
              },
              xAxis: [
                {
                  type: 'category',
                  data: this.listState.orderFullYearAxis,
                  axisTick: {
                    alignWithLabel: true
                  }
                }
              ],
              yAxis: [
                {
                  type: 'value'
                }
              ],
              series: [
                {
                  name: 'Direct',
                  type: 'bar',
                  barWidth: '60%',
                  data: this.listState.orderFullYear,
                  color:'red'
                }
              ]
            })
        }
      },
      methods:{
        setDay(){
          const day = dayjs().format("YYYY-MM-DD");
          this.date = [day, day];
        },
        //本周
        setWeek() {
          const start = dayjs().day(1).format("YYYY-MM-DD");
          const end = dayjs().day(7).format("YYYY-MM-DD");
          this.date = [start, end];
        },
        //本月
        setMonth() {
          const start = dayjs().startOf("month").format("YYYY-MM-DD");
          const end = dayjs().endOf("month").format("YYYY-MM-DD");
          this.date = [start, end];
        },
        //本年
        setYear() {
          const start = dayjs().startOf("year").format("YYYY-MM-DD");
          const end = dayjs().endOf("year").format("YYYY-MM-DD");
          this.date = [start, end];
        }
      }
    }
</script>

<style scoped>
  .clearfix{
    position: relative;
    display: flex;
    justify-content: space-between;
  }
  .tab{
    width: 100%;
  }
  .right{
    position: absolute;
    right: 0px;
  }
  .date{
    width: 250px;
    margin: 0px 20px;
  }
  .right span{
    margin: 0px 10px;
  }
  .charts{
    width: 100%;
    height: 300px;
  }
  ul {
    list-style: none;
    width: 100%;
    height: 300px;
    padding: 0px;
  }
  ul li {
    height: 8%;
    margin: 10px 0px;
  }
  .rindex {
    float: left;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: black;
    color: white;
    text-align: center;
    margin: 0px 5px;

  }
  .rvalue {
    float: right;
  }
</style>
