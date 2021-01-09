<template>
  <div>
    <!-- 面包屑导航 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>数据统计</el-breadcrumb-item>
      <el-breadcrumb-item>数据报表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图 -->
    <el-card>
      <!-- 2.为ECharts准备一个具备大小（宽高）的Dom -->
    <div id="main" style="width: 100%; height: 550px;"></div>
    </el-card>
  </div>
</template>

<script>
// 1.导入echart
import * as echarts from 'echarts'
import _ from 'lodash'
export default {
  data () {
    return {
      // 需要合并的数据
      options: {
        title: {
          text: '用户来源'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              backgroundColor: '#E9EEF3'
            }
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        xAxis: [
          { boundaryGap: false }
        ],
        yAxis: [
          { type: 'value' }
        ]
      }
    }
  },
  created () {},
  // 此时页面上元素渲染完毕
  async mounted () {
    // 3.初始化echarts实例
    let myChart = echarts.init(document.getElementById('main'));
    // 4.指定图表的配置项和数据
    const { data: res } = await this.$http.get('reports/type/1')
    if (res.meta.status !== 200) {
      return this.$message('获取报表失败')
    }
    const result = _.merge(res.data, this.options)
    // 5.使用刚指定的配置项和数据显示图表。
    myChart.setOption(result)
  },
  methods: {}
}

</script>

<style lang='less' scoped>

</style>
