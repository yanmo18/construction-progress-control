<template>
  <div class="statistical-analysis">
    <el-card shadow="hover">
      <template #header>
        <div class="card-header">
          <span>统计分析</span>
          <div class="header-actions">
            <el-select v-model="timeRange" placeholder="时间范围" size="small" style="margin-right: 10px;">
              <el-option label="近7天" value="7d" />
              <el-option label="近30天" value="30d" />
              <el-option label="近90天" value="90d" />
              <el-option label="今年" value="year" />
            </el-select>
            <el-button type="info" size="small" icon="el-icon-refresh">刷新数据</el-button>
            <el-button type="primary" icon="el-icon-download">导出报表</el-button>
          </div>
        </div>
      </template>
      
      <!-- 统计卡片 -->
      <div class="stat-cards">
        <el-row :gutter="20" style="margin-bottom: 20px;">
          <el-col :span="6">
            <el-card shadow="hover" class="stat-card" :body-style="{ padding: '20px' }">
              <div class="card-content">
                <div class="card-header-small">
                  <span class="desc">总投资</span>
                  <el-tag size="small" type="info">项目概览</el-tag>
                </div>
                <div class="number">¥2,450,000</div>
                <div class="trend" style="margin-top: 10px; font-size: 12px; color: var(--success-color);">
                  <i class="el-icon-arrow-up"></i> 较上月增长 5.2%
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="6">
            <el-card shadow="hover" class="stat-card" :body-style="{ padding: '20px' }">
              <div class="card-content">
                <div class="card-header-small">
                  <span class="desc">已完成投资</span>
                  <el-tag size="small" type="success">投资分析</el-tag>
                </div>
                <div class="number">¥1,280,000</div>
                <div class="trend" style="margin-top: 10px; font-size: 12px; color: var(--success-color);">
                  <i class="el-icon-arrow-up"></i> 占总投资 52.2%
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="6">
            <el-card shadow="hover" class="stat-card" :body-style="{ padding: '20px' }">
              <div class="card-content">
                <div class="card-header-small">
                  <span class="desc">平均进度</span>
                  <el-tag size="small" type="primary">进度管理</el-tag>
                </div>
                <div class="number">85%</div>
                <div class="trend" style="margin-top: 10px; font-size: 12px; color: var(--info-color);">
                  <i class="el-icon-arrow-right"></i> 符合计划要求
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="6">
            <el-card shadow="hover" class="stat-card" :body-style="{ padding: '20px' }">
              <div class="card-content">
                <div class="card-header-small">
                  <span class="desc">安全隐患</span>
                  <el-tag size="small" type="warning">安全管理</el-tag>
                </div>
                <div class="number" style="color: var(--warning-color);">12</div>
                <div class="trend" style="margin-top: 10px; font-size: 12px; color: var(--warning-color);">
                  <i class="el-icon-arrow-up"></i> 需立即处理
                </div>
              </div>
            </el-card>
          </el-col>
        </el-row>
      </div>
      
      <!-- 图表标签页 -->
      <el-tabs v-model="activeTab" type="card">

        <el-tab-pane label="项目进度" name="progress">
          <el-row :gutter="20">
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisProgressChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisProgressTrendChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
          </el-row>
        </el-tab-pane>

        <el-tab-pane label="资源使用" name="resource">
          <el-row :gutter="20">
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisResourceChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisResourceTrendChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="成本分析" name="cost">
          <el-row :gutter="20">
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisCostChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisCostBreakdownChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="质量安全" name="quality">
          <el-row :gutter="20">
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisQualityChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisSafetyChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="设备管理" name="equipment">
          <el-row :gutter="20">
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisEquipmentChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
            <el-col :span="12">
              <div class="chart-container" style="height: 400px;">
                <div id="analysisEquipmentStatusChart" style="width: 100%; height: 100%;"></div>
              </div>
            </el-col>
          </el-row>
        </el-tab-pane>
      </el-tabs>
    </el-card>
  </div>
</template>

<script>
import * as echarts from 'echarts'

export default {
  data() {
    return {
      activeTab: 'progress',
      timeRange: '30d',
      progressChart: null,
      progressTrendChart: null,
      resourceChart: null,
      resourceTrendChart: null,
      costChart: null,
      costBreakdownChart: null,
      qualityChart: null,
      safetyChart: null,
      equipmentChart: null,
      equipmentStatusChart: null
    }
  },
  mounted() {
    this.initProgressChart()
    this.initProgressTrendChart()
    window.addEventListener('resize', this.handleResize)
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.handleResize)
    if (this.progressChart) this.progressChart.dispose()
    if (this.progressTrendChart) this.progressTrendChart.dispose()
    if (this.resourceChart) this.resourceChart.dispose()
    if (this.resourceTrendChart) this.resourceTrendChart.dispose()
    if (this.costChart) this.costChart.dispose()
    if (this.costBreakdownChart) this.costBreakdownChart.dispose()
    if (this.qualityChart) this.qualityChart.dispose()
    if (this.safetyChart) this.safetyChart.dispose()
    if (this.equipmentChart) this.equipmentChart.dispose()
    if (this.equipmentStatusChart) this.equipmentStatusChart.dispose()
  },
  watch: {
    activeTab(newTab) {
      setTimeout(() => {
        if (newTab === 'progress') {
          this.initProgressChart()
          this.initProgressTrendChart()
        } else if (newTab === 'resource') {
          this.initResourceChart()
          this.initResourceTrendChart()
        } else if (newTab === 'cost') {
          this.initCostChart()
          this.initCostBreakdownChart()
        } else if (newTab === 'quality') {
          this.initQualityChart()
          this.initSafetyChart()
        } else if (newTab === 'equipment') {
          this.initEquipmentChart()
          this.initEquipmentStatusChart()
        }
      }, 300)
    },
    timeRange() {
      // 时间范围变化时重新加载数据
      this.loadData()
    }
  },
  methods: {
    loadData() {
      // 模拟加载数据
      console.log('加载数据:', this.timeRange)
      // 这里可以添加实际的数据加载逻辑
    },
    initProgressChart() {
      if (this.progressChart) {
        this.progressChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisProgressChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.progressChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.progressChart.clear()
            
            const option = {
              title: {
                text: '项目进度分布',
                left: 'center'
              },
              tooltip: {
                trigger: 'item',
                formatter: '{a} <br/>{b}: {c} ({d}%)'
              },
              legend: {
                orient: 'vertical',
                left: 'left'
              },
              series: [
                {
                  name: '项目进度',
                  type: 'pie',
                  radius: '60%',
                  data: [
                    { value: 5, name: '未开始', itemStyle: { color: '#999999' } },
                    { value: 15, name: '进行中', itemStyle: { color: '#666666' } },
                    { value: 4, name: '已完成', itemStyle: { color: '#333333' } },
                    { value: 2, name: '延期', itemStyle: { color: '#777777' } }
                  ],
                  itemStyle: {
                    emphasis: {
                      shadowBlur: 10,
                      shadowOffsetX: 0,
                      shadowColor: 'rgba(0, 0, 0, 0.5)'
                    }
                  }
                }
              ]
            }
            
            this.progressChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initProgressTrendChart() {
      if (this.progressTrendChart) {
        this.progressTrendChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisProgressTrendChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.progressTrendChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.progressTrendChart.clear()
            
            const option = {
              title: {
                text: '项目进度趋势',
                left: 'center'
              },
              tooltip: {
                trigger: 'axis'
              },
              legend: {
                data: ['计划进度', '实际进度'],
                bottom: 0
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '15%',
                top: '15%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                boundaryGap: false,
                data: ['1月', '2月', '3月', '4月', '5月', '6月']
              },
              yAxis: {
                type: 'value',
                max: 100,
                axisLabel: {
                  formatter: '{value}%'
                }
              },
              series: [
                {
                  name: '计划进度',
                  type: 'line',
                  data: [20, 40, 60, 75, 90, 100],
                  smooth: true,
                  lineStyle: {
                    width: 3,
                    color: '#333333'
                  },
                  itemStyle: {
                    color: '#333333'
                  }
                },
                {
                  name: '实际进度',
                  type: 'line',
                  data: [15, 35, 55, 70, 85, 95],
                  smooth: true,
                  lineStyle: {
                    width: 3,
                    color: '#666666'
                  },
                  itemStyle: {
                    color: '#666666'
                  }
                }
              ]
            }
            
            this.progressTrendChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initResourceChart() {
      if (this.resourceChart) {
        this.resourceChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisResourceChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.resourceChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.resourceChart.clear()
            
            const option = {
              title: {
                text: '资源使用情况',
                left: 'center'
              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                data: ['人力', '材料', '设备'],
                bottom: 0
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '15%',
                top: '15%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                data: ['项目A', '项目B', '项目C', '项目D', '项目E']
              },
              yAxis: {
                type: 'value'
              },
              series: [
                {
                  name: '人力',
                  type: 'bar',
                  data: [50, 40, 60, 30, 45]
                },
                {
                  name: '材料',
                  type: 'bar',
                  data: [120, 90, 150, 80, 100]
                },
                {
                  name: '设备',
                  type: 'bar',
                  data: [20, 15, 25, 10, 18]
                }
              ]
            }
            
            this.resourceChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initResourceTrendChart() {
      if (this.resourceTrendChart) {
        this.resourceTrendChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisResourceTrendChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.resourceTrendChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.resourceTrendChart.clear()
            
            const option = {
              title: {
                text: '资源使用趋势',
                left: 'center'
              },
              tooltip: {
                trigger: 'axis'
              },
              legend: {
                data: ['人力', '材料', '设备'],
                bottom: 0
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '15%',
                top: '15%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                boundaryGap: false,
                data: ['1月', '2月', '3月', '4月', '5月', '6月']
              },
              yAxis: {
                type: 'value'
              },
              series: [
                {
                  name: '人力',
                  type: 'line',
                  data: [40, 45, 50, 48, 55, 60],
                  smooth: true
                },
                {
                  name: '材料',
                  type: 'line',
                  data: [80, 90, 110, 100, 120, 130],
                  smooth: true
                },
                {
                  name: '设备',
                  type: 'line',
                  data: [15, 18, 20, 19, 22, 25],
                  smooth: true
                }
              ]
            }
            
            this.resourceTrendChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initCostChart() {
      if (this.costChart) {
        this.costChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisCostChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.costChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.costChart.clear()
            
            const option = {
              title: {
                text: '项目成本分析',
                left: 'center'
              },
              tooltip: {
                trigger: 'axis'
              },
              legend: {
                data: ['计划成本', '实际成本'],
                bottom: 0
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '15%',
                top: '15%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                boundaryGap: false,
                data: ['1月', '2月', '3月', '4月', '5月', '6月']
              },
              yAxis: {
                type: 'value'
              },
              series: [
                {
                  name: '计划成本',
                  type: 'line',
                  data: [120, 132, 101, 134, 90, 230],
                  smooth: true
                },
                {
                  name: '实际成本',
                  type: 'line',
                  data: [110, 128, 105, 130, 95, 220],
                  smooth: true
                }
              ]
            }
            
            this.costChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initCostBreakdownChart() {
      if (this.costBreakdownChart) {
        this.costBreakdownChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisCostBreakdownChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.costBreakdownChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.costBreakdownChart.clear()
            
            const option = {
              title: {
                text: '成本构成分析',
                left: 'center'
              },
              tooltip: {
                trigger: 'item',
                formatter: '{a} <br/>{b}: {c} ({d}%)'
              },
              legend: {
                orient: 'vertical',
                left: 'left'
              },
              series: [
                {
                  name: '成本构成',
                  type: 'pie',
                  radius: '60%',
                  data: [
                    { value: 800000, name: '人工费' },
                    { value: 1200000, name: '材料费' },
                    { value: 300000, name: '设备费' },
                    { value: 150000, name: '其他费用' }
                  ],
                  itemStyle: {
                    emphasis: {
                      shadowBlur: 10,
                      shadowOffsetX: 0,
                      shadowColor: 'rgba(0, 0, 0, 0.5)'
                    }
                  }
                }
              ]
            }
            
            this.costBreakdownChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initQualityChart() {
      if (this.qualityChart) {
        this.qualityChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisQualityChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.qualityChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.qualityChart.clear()
            
            const option = {
              title: {
                text: '质量安全问题统计',
                left: 'center'
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
                top: '15%',
                containLabel: true
              },
              xAxis: {
                type: 'value'
              },
              yAxis: {
                type: 'category',
                data: ['安全隐患', '质量问题', '文明施工', '设备故障', '材料问题']
              },
              series: [
                {
                  name: '问题数量',
                  type: 'bar',
                  data: [12, 8, 5, 3, 6],
                  itemStyle: {
                    color: '#666666'
                  },
                  label: {
                    show: true,
                    position: 'right',
                    formatter: '{c} 个',
                    color: '#333333'
                  }
                }
              ]
            }
            
            this.qualityChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initSafetyChart() {
      if (this.safetyChart) {
        this.safetyChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisSafetyChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.safetyChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.safetyChart.clear()
            
            const option = {
              title: {
                text: '安全隐患趋势',
                left: 'center'
              },
              tooltip: {
                trigger: 'axis'
              },
              legend: {
                data: ['安全隐患', '已处理'],
                bottom: 0
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '15%',
                top: '15%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                boundaryGap: false,
                data: ['1月', '2月', '3月', '4月', '5月', '6月']
              },
              yAxis: {
                type: 'value'
              },
              series: [
                {
                  name: '安全隐患',
                  type: 'line',
                  data: [15, 12, 18, 10, 14, 12],
                  smooth: true
                },
                {
                  name: '已处理',
                  type: 'line',
                  data: [13, 10, 16, 9, 12, 11],
                  smooth: true
                }
              ]
            }
            
            this.safetyChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initEquipmentChart() {
      if (this.equipmentChart) {
        this.equipmentChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisEquipmentChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.equipmentChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.equipmentChart.clear()
            
            const option = {
              title: {
                text: '设备使用情况',
                left: 'center'
              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                data: ['设备数量', '使用中'],
                bottom: 0
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '15%',
                top: '15%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                data: ['塔吊', '挖掘机', '装载机', '混凝土泵车', '运输车']
              },
              yAxis: {
                type: 'value'
              },
              series: [
                {
                  name: '设备数量',
                  type: 'bar',
                  data: [5, 8, 6, 3, 10]
                },
                {
                  name: '使用中',
                  type: 'bar',
                  data: [4, 6, 5, 2, 8]
                }
              ]
            }
            
            this.equipmentChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    initEquipmentStatusChart() {
      if (this.equipmentStatusChart) {
        this.equipmentStatusChart.dispose()
      }
      
      setTimeout(() => {
        const chartDom = document.getElementById('analysisEquipmentStatusChart')
        if (chartDom) {
          // 确保图表容器有正确的尺寸
          chartDom.style.width = '100%'
          chartDom.style.height = '100%'
          
          // 强制重排，确保获取正确的尺寸
          chartDom.offsetHeight // 触发重排
          
          try {
            this.equipmentStatusChart = echarts.init(chartDom)
            
            // 清空之前的配置
            this.equipmentStatusChart.clear()
            
            const option = {
              title: {
                text: '设备状态分布',
                left: 'center'
              },
              tooltip: {
                trigger: 'item',
                formatter: '{a} <br/>{b}: {c} ({d}%)'
              },
              legend: {
                orient: 'vertical',
                left: 'left'
              },
              series: [
                {
                  name: '设备状态',
                  type: 'pie',
                  radius: '60%',
                  data: [
                    { value: 25, name: '正常运行' },
                    { value: 5, name: '维护中' },
                    { value: 2, name: '故障' },
                    { value: 3, name: '闲置' }
                  ],
                  itemStyle: {
                    emphasis: {
                      shadowBlur: 10,
                      shadowOffsetX: 0,
                      shadowColor: 'rgba(0, 0, 0, 0.5)'
                    }
                  }
                }
              ]
            }
            
            this.equipmentStatusChart.setOption(option)
          } catch (error) {
            console.warn('ECharts initialization error:', error)
          }
        }
      }, 200)
    },
    handleResize() {
      try {
        if (this.progressChart) {
          const chartDom = document.getElementById('analysisProgressChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.progressChart.resize()
          }
        }
        if (this.progressTrendChart) {
          const chartDom = document.getElementById('analysisProgressTrendChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.progressTrendChart.resize()
          }
        }
        if (this.resourceChart) {
          const chartDom = document.getElementById('analysisResourceChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.resourceChart.resize()
          }
        }
        if (this.resourceTrendChart) {
          const chartDom = document.getElementById('analysisResourceTrendChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.resourceTrendChart.resize()
          }
        }
        if (this.costChart) {
          const chartDom = document.getElementById('analysisCostChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.costChart.resize()
          }
        }
        if (this.costBreakdownChart) {
          const chartDom = document.getElementById('analysisCostBreakdownChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.costBreakdownChart.resize()
          }
        }
        if (this.qualityChart) {
          const chartDom = document.getElementById('analysisQualityChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.qualityChart.resize()
          }
        }
        if (this.safetyChart) {
          const chartDom = document.getElementById('analysisSafetyChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.safetyChart.resize()
          }
        }
        if (this.equipmentChart) {
          const chartDom = document.getElementById('analysisEquipmentChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.equipmentChart.resize()
          }
        }
        if (this.equipmentStatusChart) {
          const chartDom = document.getElementById('analysisEquipmentStatusChart')
          if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
            this.equipmentStatusChart.resize()
          }
        }
      } catch (error) {
        console.warn('ECharts resize error:', error)
      }
    }
  }
}
</script>

<style scoped>
.statistical-analysis {
  width: 100%;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 10px;
}

.header-actions .el-button {
  font-size: 12px;
  padding: 6px 12px;
  border-radius: 4px;
}

.stat-cards {
  margin-bottom: 30px;
}

.chart-container {
  width: 100%;
  height: 400px;
  margin-top: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  padding: 20px;
  box-sizing: border-box;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.card-content {
  text-align: center;
  padding: 24px 0;
  transition: all 0.3s ease;
}

.card-content:hover {
  transform: translateY(-2px);
}

.number {
  font-size: 28px;
  font-weight: bold;
  color: #333333;
  margin: 10px 0;
}

.trend {
  font-size: 12px;
  margin-top: 10px;
}

.card-header-small {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
  padding: 0 10px;
}

.card-header-small .desc {
  margin: 0;
  font-size: 14px;
  font-weight: 500;
  color: #333333;
}

/* 标签页样式 */
.el-tabs__header {
  margin-bottom: 20px;
}

.el-tabs__item {
  font-size: 14px;
  font-weight: 500;
  color: #666666;
}

.el-tabs__item.is-active {
  color: #333333;
  font-weight: 600;
}

/* 统计卡片样式 */
.stat-card {
  border-radius: 8px;
  transition: all 0.3s ease;
  border: 1px solid #e8e8e8;
}

.stat-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  border-color: #d9d9d9;
}

/* 响应式调整 */
@media screen and (max-width: 1200px) {
  .el-col {
    margin-bottom: 20px;
  }
}
</style>