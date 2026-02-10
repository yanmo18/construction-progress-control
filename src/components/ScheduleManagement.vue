<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import * as echarts from 'echarts'

// 响应式数据
const scheduleList = ref([
  {
    id: 1,
    projectName: '项目A',
    phase: '基础工程',
    plannedStart: '2024-01-01',
    plannedEnd: '2024-02-15',
    actualStart: '2024-01-01',
    actualEnd: '2024-02-10',
    progress: 95,
    status: '进行中',
    manager: '张三'
  },
  {
    id: 2,
    projectName: '项目A',
    phase: '主体结构',
    plannedStart: '2024-02-16',
    plannedEnd: '2024-05-15',
    actualStart: '2024-02-12',
    actualEnd: null,
    progress: 30,
    status: '进行中',
    manager: '李四'
  },
  {
    id: 3,
    projectName: '项目B',
    phase: '基础工程',
    plannedStart: '2024-01-10',
    plannedEnd: '2024-02-28',
    actualStart: '2024-01-10',
    actualEnd: '2024-03-05',
    progress: 100,
    status: '已完成',
    manager: '王五'
  },
  {
    id: 4,
    projectName: '项目B',
    phase: '主体结构',
    plannedStart: '2024-03-01',
    plannedEnd: '2024-06-01',
    actualStart: '2024-03-06',
    actualEnd: null,
    progress: 20,
    status: '进行中',
    manager: '赵六'
  },
  {
    id: 5,
    projectName: '项目C',
    phase: '基础工程',
    plannedStart: '2024-01-15',
    plannedEnd: '2024-03-15',
    actualStart: null,
    actualEnd: null,
    progress: 0,
    status: '未开始',
    manager: '孙七'
  }
])

const ganttChart = ref(null)
const searchQuery = ref('')
const filterStatus = ref('')
const dateRange = ref([])
const currentPage = ref(1)
const pageSize = ref(10)
const showProgressCompare = ref(false)

// 右侧面板数据
const statusStats = ref([
  { status: '未开始', count: 1, percentage: 20, color: '#1890ff' },
  { status: '进行中', count: 3, percentage: 60, color: '#faad14' },
  { status: '已完成', count: 1, percentage: 20, color: '#52c41a' }
])

const progressAlerts = ref([
  {
    id: 1,
    title: '进度延迟',
    type: 'warning',
    description: '项目B主体结构进度延迟10天'
  },
  {
    id: 2,
    title: '即将到期',
    type: 'info',
    description: '项目A主体结构将在5天后到期'
  }
])

// 初始化甘特图
const initGanttChart = () => {
  if (ganttChart.value) {
    ganttChart.value.dispose()
  }
  
  setTimeout(() => {
    const chartDom = document.getElementById('ganttChart')
    if (chartDom) {
      // 确保图表容器有正确的尺寸
      chartDom.style.width = '100%'
      chartDom.style.height = '400px'
      
      // 强制重排，确保获取正确的尺寸
      chartDom.offsetHeight // 触发重排
      
      try {
        ganttChart.value = echarts.init(chartDom)
        
        const option = {
          title: {
            text: '项目进度趋势图',
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
        
        ganttChart.value.setOption(option)
        
        // 添加图表点击事件
        ganttChart.value.on('click', function(params) {
          console.log('点击了图表:', params)
          // 可以在这里添加点击事件处理逻辑
        })
      } catch (error) {
        console.warn('ECharts initialization error:', error)
      }
    }
  }, 300)
}

// 处理图表 resize
const handleResize = () => {
  if (ganttChart.value) {
    const chartDom = document.getElementById('ganttChart')
    if (chartDom && chartDom.parentElement && chartDom.parentElement.clientWidth > 0) {
      ganttChart.value.resize()
    }
  }
}

// 分页方法
const handleSizeChange = (size) => {
  pageSize.value = size
  currentPage.value = 1
}

const handleCurrentChange = (current) => {
  currentPage.value = current
}

// 搜索和筛选方法
const handleSearch = () => {
  console.log('搜索:', searchQuery.value)
  // 在这里添加搜索逻辑
}

const handleReset = () => {
  searchQuery.value = ''
  filterStatus.value = ''
  dateRange.value = []
}

// 进度对比方法
const toggleProgressCompare = () => {
  showProgressCompare.value = !showProgressCompare.value
}

// 右侧面板快速操作方法
const batchImport = () => {
  console.log('批量导入进度数据')
  // 这里可以添加批量导入逻辑
}

const exportTemplate = () => {
  console.log('导出进度模板')
  // 这里可以添加导出模板逻辑
}

const printReport = () => {
  console.log('打印进度报表')
  // 这里可以添加打印报表逻辑
}

const scheduleMeeting = () => {
  console.log('安排进度会议')
  // 这里可以添加安排会议逻辑
}

// 生命周期钩子
onMounted(() => {
  initGanttChart()
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  if (ganttChart.value) {
    ganttChart.value.dispose()
  }
})
</script>

<template>
  <div class="schedule-management">
    <div class="main-content">
      <!-- 左侧主要内容 -->
      <div class="left-content">
        <el-card shadow="hover">
          <template #header>
            <div class="card-header">
              <span>施工进度管理</span>
              <div class="header-actions">
                <el-button type="info" icon="el-icon-data-analysis" @click="toggleProgressCompare">
                  {{ showProgressCompare ? '隐藏对比' : '进度对比' }}
                </el-button>
                <el-button type="primary" icon="el-icon-plus">添加进度</el-button>
              </div>
            </div>
          </template>
          
          <!-- 搜索和筛选 -->
          <div class="search-filter" style="margin-bottom: 20px;">
            <el-row :gutter="20">
              <el-col :span="6">
                <el-input 
                  v-model="searchQuery"
                  placeholder="项目名称" 
                  prefix-icon="el-icon-search"
                ></el-input>
              </el-col>
              <el-col :span="6">
                <el-select 
                  v-model="filterStatus"
                  placeholder="状态" 
                  style="width: 100%"
                >
                  <el-option label="全部" value=""></el-option>
                  <el-option label="未开始" value="未开始"></el-option>
                  <el-option label="进行中" value="进行中"></el-option>
                  <el-option label="已完成" value="已完成"></el-option>
                </el-select>
              </el-col>
              <el-col :span="8">
                <el-date-picker
                  v-model="dateRange"
                  type="daterange"
                  range-separator="至"
                  start-placeholder="开始日期"
                  end-placeholder="结束日期"
                  style="width: 100%"
                ></el-date-picker>
              </el-col>
              <el-col :span="4">
                <el-button type="primary" icon="el-icon-search" @click="handleSearch">搜索</el-button>
                <el-button icon="el-icon-refresh" @click="handleReset">重置</el-button>
              </el-col>
            </el-row>
          </div>
          
          <!-- 进度列表 -->
          <div class="schedule-list" style="margin-bottom: 20px;">
            <el-table :data="scheduleList" style="width: 100%" stripe>
              <el-table-column prop="id" label="ID" width="80"></el-table-column>
              <el-table-column prop="projectName" label="项目名称" width="120"></el-table-column>
              <el-table-column prop="phase" label="施工阶段" width="120"></el-table-column>
              <el-table-column prop="plannedStart" label="计划开始" width="120"></el-table-column>
              <el-table-column prop="plannedEnd" label="计划结束" width="120"></el-table-column>
              <el-table-column prop="actualStart" label="实际开始" width="120"></el-table-column>
              <el-table-column prop="actualEnd" label="实际结束" width="120"></el-table-column>
              <el-table-column prop="progress" label="进度" width="180">
                <template #default="scope">
                  <el-progress 
                    :percentage="scope.row.progress" 
                    :color="scope.row.progress >= 100 ? '#52c41a' : scope.row.progress >= 50 ? '#1890ff' : '#faad14'" 
                    :stroke-width="10"
                  ></el-progress>
                </template>
              </el-table-column>
              <el-table-column prop="status" label="状态" width="100">
                <template #default="scope">
                  <el-tag 
                    :type="scope.row.status === '已完成' ? 'success' : (scope.row.status === '进行中' ? 'primary' : 'info')"
                  >
                    {{ scope.row.status }}
                  </el-tag>
                </template>
              </el-table-column>
              <el-table-column prop="manager" label="负责人" width="100"></el-table-column>
              <el-table-column label="操作" width="180" fixed="right">
                <template #default>
                  <div class="table-actions">
                    <el-button size="mini" type="primary" icon="el-icon-view">查看</el-button>
                    <el-button size="mini" type="success" icon="el-icon-edit">编辑</el-button>
                    <el-button size="mini" type="danger" icon="el-icon-delete">删除</el-button>
                  </div>
                </template>
              </el-table-column>
            </el-table>
          </div>
          
          <!-- 分页 -->
          <div class="pagination" style="display: flex; justify-content: flex-end; margin-top: 16px;">
            <el-pagination
              layout="prev, pager, next, jumper, ->, total"
              :total="scheduleList.length"
              :page-size="pageSize"
              :current-page="currentPage"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
            ></el-pagination>
          </div>
        </el-card>
        
        <!-- 进度对比功能 -->
        <el-card v-if="showProgressCompare" shadow="hover" style="margin-top: 20px;">
          <template #header>
            <div class="card-header">
              <span>进度对比分析</span>
            </div>
          </template>
          <div class="progress-compare">
            <el-row :gutter="20">
              <el-col :span="12">
                <h4 style="color: var(--text-secondary); margin-bottom: 16px;">计划 vs 实际进度对比</h4>
                <div style="height: 300px;">
                  <!-- 这里可以添加对比图表 -->
                </div>
              </el-col>
              <el-col :span="12">
                <h4 style="color: var(--text-secondary); margin-bottom: 16px;">进度偏差分析</h4>
                <div style="height: 300px;">
                  <!-- 这里可以添加偏差分析图表 -->
                </div>
              </el-col>
            </el-row>
          </div>
        </el-card>
        
        <!-- 项目进度趋势图 -->
        <el-card shadow="hover" style="margin-top: 20px;">
          <template #header>
            <div class="card-header">
              <span>项目进度趋势图</span>
              <el-button size="small" type="info" icon="el-icon-download">导出</el-button>
            </div>
          </template>
          <div class="gantt-container" style="height: 400px;">
            <div id="ganttChart" style="width: 100%; height: 100%;"></div>
          </div>
        </el-card>
      </div>
      
      <!-- 右侧功能面板 -->
      <div class="right-panel">
        <el-card shadow="hover" class="right-card">
          <template #header>
            <div class="card-header">
              <span>进度概览</span>
            </div>
          </template>
          <div class="overview-content">
            <!-- 项目状态统计 -->
            <div class="status-stats">
              <h4 style="margin-bottom: 16px; font-size: 14px; font-weight: 500;">项目状态分布</h4>
              <div class="status-item" v-for="item in statusStats" :key="item.status">
                <div class="status-info">
                  <span class="status-name">{{ item.status }}</span>
                  <span class="status-count">{{ item.count }}</span>
                </div>
                <el-progress 
                  :percentage="item.percentage" 
                  :color="item.color"
                  :stroke-width="8"
                  :show-text="false"
                ></el-progress>
              </div>
            </div>
            
            <!-- 进度预警 -->
            <div class="progress-alerts" style="margin-top: 24px;">
              <h4 style="margin-bottom: 16px; font-size: 14px; font-weight: 500;">进度预警</h4>
              <div class="alert-item" v-for="alert in progressAlerts" :key="alert.id">
                <el-alert
                  :title="alert.title"
                  :type="alert.type"
                  :description="alert.description"
                  show-icon
                  :closable="false"
                  style="margin-bottom: 10px;"
                >
                </el-alert>
              </div>
            </div>
            
            <!-- 快速操作 -->
            <div style="margin-top: 24px;">
              <h4 style="margin-bottom: 16px; font-size: 14px; font-weight: 500;">快速操作</h4>
              <div style="display: flex; flex-direction: column; gap: 10px;">
                <button 
                  class="action-button primary" 
                  @click="batchImport"
                >
                  批量导入
                </button>
                <button 
                  class="action-button success" 
                  @click="exportTemplate"
                >
                  导出模板
                </button>
                <button 
                  class="action-button info" 
                  @click="printReport"
                >
                  打印报表
                </button>
                <button 
                  class="action-button warning" 
                  @click="scheduleMeeting"
                >
                  进度会议
                </button>
              </div>
            </div>
          </div>
        </el-card>
      </div>
    </div>
  </div>
</template>

<style scoped>
.schedule-management {
  width: 100%;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
}

.main-content {
  display: flex;
  gap: 20px;
  width: 100%;
}

.left-content {
  flex: 1;
  min-width: 0;
}

.right-panel {
  width: 320px;
  flex-shrink: 0;
}

.right-card {
  height: fit-content;
  position: sticky;
  top: 20px;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

.header-actions {
  display: flex;
  gap: 10px;
}

.header-actions .el-button {
  font-size: 12px;
  padding: 6px 12px;
  border-radius: 4px;
}

.search-filter {
  margin-bottom: 20px;
}

.schedule-list {
  margin-bottom: 20px;
}

.pagination {
  display: flex;
  justify-content: flex-end;
  margin-top: 20px;
}

.gantt-container {
  width: 100%;
  height: 400px;
}

.table-actions {
  display: flex;
  gap: 4px;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.table-actions .el-button {
  min-width: 50px;
  text-align: center;
  font-size: 12px;
  padding: 4px 8px;
}

/* 右侧面板样式 */
.overview-content {
  padding: 10px;
}

.status-stats {
  margin-bottom: 20px;
}

.status-item {
  margin-bottom: 16px;
}

.status-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
  font-size: 13px;
}

.status-name {
  color: #333;
  font-weight: 500;
}

.status-count {
  color: #666;
  font-size: 12px;
}

.progress-alerts {
  margin-bottom: 20px;
}

.alert-item {
  margin-bottom: 10px;
}

/* 快速操作按钮样式 */
.action-button {
  width: 100%;
  height: 40px;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 0;
  font-family: inherit;
}

.action-button:hover {
  opacity: 0.9;
  transform: translateY(-1px);
}

.action-button.primary {
  background-color: #1890ff;
}

.action-button.success {
  background-color: #52c41a;
}

.action-button.info {
  background-color: #6c757d;
}

.action-button.warning {
  background-color: #ffc107;
  color: #333333;
}

/* 确保按钮组容器没有额外的内边距或边框 */
.right-card .el-card__body {
  padding: 20px;
  box-sizing: border-box;
}

.overview-content {
  padding: 0;
  width: 100%;
  box-sizing: border-box;
}

/* 响应式设计 */
@media screen and (max-width: 1200px) {
  .main-content {
    flex-direction: column;
  }
  
  .right-panel {
    width: 100%;
  }
  
  .right-card {
    position: static;
  }
}

@media screen and (max-width: 768px) {
  .schedule-management {
    padding: 10px;
  }
  
  .header-actions {
    flex-direction: column;
    gap: 5px;
  }
  
  .header-actions .el-button {
    width: 100%;
  }
}
</style>