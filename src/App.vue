<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue'
import * as echarts from 'echarts'
import ScheduleManagement from './components/ScheduleManagement.vue'
import StatisticalAnalysis from './components/StatsAnalysis.vue'
import UserManagement from './components/UserManagement.vue'
import DocumentManagement from './components/DocumentManagement.vue'
import RemoteMonitoring from './components/RemoteMonitoring.vue'
import SystemSettings from './components/SystemSettings.vue'

const activeMenu = ref('dashboard')
const isCollapse = ref(false)
const progressChart = ref(null)
const searchQuery = ref('')
const filterStatus = ref('all')
const chartType = ref('bar')

const recentProjects = ref([
  {
    id: 1,
    name: '项目A',
    phase: '基础工程',
    progress: 95,
    status: '进行中',
    manager: '张三',
    deadline: '2024-02-28'
  },
  {
    id: 2,
    name: '项目B',
    phase: '主体结构',
    progress: 30,
    status: '进行中',
    manager: '李四',
    deadline: '2024-05-15'
  },
  {
    id: 3,
    name: '项目C',
    phase: '基础工程',
    progress: 100,
    status: '已完成',
    manager: '王五',
    deadline: '2024-02-28'
  },
  {
    id: 4,
    name: '项目D',
    phase: '主体结构',
    progress: 20,
    status: '进行中',
    manager: '赵六',
    deadline: '2024-06-01'
  },
  {
    id: 5,
    name: '项目E',
    phase: '装修工程',
    progress: 75,
    status: '进行中',
    manager: '孙七',
    deadline: '2024-03-31'
  }
])

const menuItems = [
  { index: 'dashboard', title: '首页', icon: 'el-icon-s-home' },
  { index: 'schedule', title: '进度管理', icon: 'el-icon-time' },
  { index: 'document', title: '文档管理', icon: 'el-icon-folder' },
  { index: 'analysis', title: '统计分析', icon: 'el-icon-data-analysis' },
  { index: 'monitor', title: '实时监控', icon: 'el-icon-video-camera' },
  { index: 'user', title: '用户管理', icon: 'el-icon-user' },
  { index: 'settings', title: '系统设置', icon: 'el-icon-setting' }
]

const initCharts = () => {
  // 初始化项目进度概览图表
  if (progressChart.value) {
    progressChart.value.dispose()
  }
  
  // 使用setTimeout确保DOM渲染完成
  setTimeout(() => {
    const chartDom = document.getElementById('progressChart')
    if (chartDom) {
      // 确保图表容器有正确的尺寸
      chartDom.style.width = '100%'
      chartDom.style.height = '300px'
      
      // 强制重排，确保获取正确的尺寸
      chartDom.offsetHeight // 触发重排
      
      try {
        progressChart.value = echarts.init(chartDom)
        
        // 清空之前的配置
        progressChart.value.clear()
        
        const option = {
          title: {
            text: '项目进度概览',
            left: 'center'
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'shadow'
            }
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
            data: ['项目A', '项目B', '项目C', '项目D', '项目E', '项目F']
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
              type: 'bar',
              data: [80, 60, 90, 50, 70, 85],
              itemStyle: {
                color: '#666666'
              }
            },
            {
              name: '实际进度',
              type: 'bar',
              data: [75, 55, 85, 45, 65, 80],
              itemStyle: {
                color: '#999999'
              }
            }
          ]
        }
        
        // 一次性设置配置，简化代码
        progressChart.value.setOption(option)
        
        // 确保图表显示，添加错误处理
        setTimeout(() => {
          try {
            progressChart.value.resize()
          } catch (resizeError) {
            console.warn('ECharts init resize error:', resizeError)
          }
        }, 200)
      } catch (error) {
        console.warn('ECharts initialization error:', error)
      }
    }
  }, 300)
}

const handleResize = () => {
  if (progressChart.value) {
    try {
      // 确保在调整大小时图表容器有正确的尺寸
      const chartDom = document.getElementById('progressChart')
      if (chartDom) {
        const container = chartDom.parentElement
        if (container) {
          const { clientWidth, clientHeight } = container
          if (clientWidth > 0 && clientHeight > 0) {
            // 延迟执行resize，确保坐标系完全初始化
            setTimeout(() => {
              try {
                progressChart.value.resize()
              } catch (resizeError) {
                console.warn('ECharts resize error:', resizeError)
              }
            }, 100)
          }
        }
      }
    } catch (error) {
      console.warn('ECharts resize error:', error)
    }
  }
}

onMounted(() => {
  initCharts()
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  if (progressChart.value) {
    progressChart.value.dispose()
  }
})

// 监听菜单切换，当切回首页时重新初始化图表
watch(activeMenu, (newMenu) => {
  if (newMenu === 'dashboard') {
    // 延迟执行，确保DOM已渲染
    setTimeout(() => {
      initCharts()
    }, 100)
  }
})
</script>

<template>
  <div class="app-container">
    <!-- 顶部导航栏 -->
    <el-header height="60px" class="header">
      <div class="header-left">
        <el-button type="primary" @click="isCollapse = !isCollapse" icon="el-icon-s-fold" circle>
        </el-button>
        <h1 class="system-title">建筑施工进度数字化管控系统</h1>
      </div>
      <div class="header-right">
        <el-dropdown>
          <span class="user-info">
            <el-avatar size="small">管理员</el-avatar>
            <span>管理员</span>
          </span>
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item>个人中心</el-dropdown-item>
              <el-dropdown-item>修改密码</el-dropdown-item>
              <el-dropdown-item divided>退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </div>
    </el-header>
    
    <div class="main-container">
      <!-- 左侧菜单栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'" class="aside el-aside" :class="{ 'is-collapse': isCollapse }">
        <el-menu
          :default-active="activeMenu"
          class="el-menu-vertical-demo"
          :collapse="isCollapse"
          background-color="#001529"
          text-color="#fff"
          active-text-color="#409EFF"
          @select="(key) => activeMenu = key"
        >
          <el-menu-item v-for="item in menuItems" :key="item.index" :index="item.index">
            <template #icon>
              <i :class="item.icon"></i>
            </template>
            <template #title>{{ item.title }}</template>
          </el-menu-item>
        </el-menu>
      </el-aside>
      
      <!-- 主内容区 -->
      <el-main class="main el-main">
        <!-- 首页内容 -->
        <div v-if="activeMenu === 'dashboard'" class="dashboard">
          <!-- 搜索和筛选栏 -->
          <div class="dashboard-header" style="margin-bottom: 20px; display: flex; justify-content: space-between; align-items: center;">
            <h2 style="color: var(--text-color); margin: 0;">项目概览</h2>
            <div class="dashboard-actions">
              <el-input
                v-model="searchQuery"
                placeholder="搜索项目"
                style="width: 240px; margin-right: 10px;"
                prefix-icon="el-icon-search"
              />
              <el-select
                v-model="filterStatus"
                placeholder="筛选状态"
                style="width: 120px; margin-right: 10px;"
              >
                <el-option label="全部" value="all" />
                <el-option label="进行中" value="进行中" />
                <el-option label="已完成" value="已完成" />
                <el-option label="延期" value="延期" />
              </el-select>
              <el-button type="primary" icon="el-icon-plus">新建项目</el-button>
            </div>
          </div>

          <!-- 统计卡片 -->
          <el-row :gutter="20">
            <el-col :span="6">
              <el-card shadow="hover" class="stat-card">
                <template #header>
                  <div class="card-header">
                    <span>项目总数</span>
                    <el-button type="primary" size="small" icon="el-icon-plus">新建</el-button>
                  </div>
                </template>
                <div class="card-content">
                  <div class="number">24</div>
                  <div class="desc">个项目</div>
                  <div class="trend" style="margin-top: 10px; font-size: 12px; color: var(--success-color);">
                    <i class="el-icon-arrow-up"></i> 较上月增长 12%
                  </div>
                </div>
              </el-card>
            </el-col>
            <el-col :span="6">
              <el-card shadow="hover" class="stat-card">
                <template #header>
                  <div class="card-header">
                    <span>进行中项目</span>
                    <el-button type="info" size="small" icon="el-icon-view">查看</el-button>
                  </div>
                </template>
                <div class="card-content">
                  <div class="number">18</div>
                  <div class="desc">个项目</div>
                  <div class="trend" style="margin-top: 10px; font-size: 12px; color: var(--info-color);">
                    <i class="el-icon-time"></i> 75% 的项目正在进行
                  </div>
                </div>
              </el-card>
            </el-col>
            <el-col :span="6">
              <el-card shadow="hover" class="stat-card">
                <template #header>
                  <div class="card-header">
                    <span>已完成项目</span>
                    <el-button type="success" size="small" icon="el-icon-check">查看</el-button>
                  </div>
                </template>
                <div class="card-content">
                  <div class="number">6</div>
                  <div class="desc">个项目</div>
                  <div class="trend" style="margin-top: 10px; font-size: 12px; color: var(--success-color);">
                    <i class="el-icon-arrow-up"></i> 较上月增长 8%
                  </div>
                </div>
              </el-card>
            </el-col>
            <el-col :span="6">
              <el-card shadow="hover" class="stat-card">
                <template #header>
                  <div class="card-header">
                    <span>延期项目</span>
                    <el-button type="warning" size="small" icon="el-icon-warning">处理</el-button>
                  </div>
                </template>
                <div class="card-content">
                  <div class="number" style="color: var(--warning-color);">2</div>
                  <div class="desc">个项目</div>
                  <div class="trend" style="margin-top: 10px; font-size: 12px; color: var(--warning-color);">
                    <i class="el-icon-arrow-down"></i> 需要立即处理
                  </div>
                </div>
              </el-card>
            </el-col>
          </el-row>
          
          <!-- 图表和任务 -->
          <el-row :gutter="20" style="margin-top: 20px;">
            <el-col :span="12">
              <el-card shadow="hover">
                <template #header>
                  <div class="card-header">
                    <span>项目进度概览</span>
                    <el-select
                      v-model="chartType"
                      size="small"
                      style="width: 120px;"
                    >
                      <el-option label="柱状图" value="bar" />
                      <el-option label="折线图" value="line" />
                    </el-select>
                  </div>
                </template>
                <div class="chart-container" style="height: 300px; width: 100%;">
                  <div id="progressChart" style="width: 100%; height: 100%;"></div>
                </div>
              </el-card>
            </el-col>
            <el-col :span="12">
              <el-card shadow="hover">
                <template #header>
                  <div class="card-header">
                    <span>近期任务</span>
                    <el-button type="info" size="small" icon="el-icon-calendar">查看日历</el-button>
                  </div>
                </template>
                <div class="task-list">
                  <el-timeline>
                    <el-timeline-item
                      v-for="(task, index) in [
                        { title: '项目A基础工程验收', time: '2024-01-15', status: 'success', progress: 100 },
                        { title: '项目B主体结构施工', time: '2024-01-18', status: 'warning', progress: 60 },
                        { title: '项目C材料进场检查', time: '2024-01-20', status: 'primary', progress: 30 },
                        { title: '项目D安全隐患排查', time: '2024-01-22', status: 'danger', progress: 0 }
                      ]"
                      :key="index"
                      :timestamp="task.time"
                      :type="task.status"
                      :icon="task.status === 'success' ? 'el-icon-check' : task.status === 'warning' ? 'el-icon-warning' : task.status === 'danger' ? 'el-icon-error' : 'el-icon-info'"
                    >
                      <div class="task-item">
                        <div class="task-title">{{ task.title }}</div>
                        <el-progress 
                          :percentage="task.progress" 
                          :color="task.status === 'success' ? '#52c41a' : task.status === 'danger' ? '#f5222d' : '#1890ff'"
                          :stroke-width="6" 
                          style="margin-top: 5px; width: 80%;"
                        />
                      </div>
                    </el-timeline-item>
                  </el-timeline>
                </div>
              </el-card>
            </el-col>
          </el-row>

          <!-- 最近项目列表 -->
          <el-row :gutter="20" style="margin-top: 20px;">
            <el-col :span="24">
              <el-card shadow="hover">
                <template #header>
                  <div class="card-header">
                    <span>最近项目</span>
                    <el-button type="info" size="small" icon="el-icon-s-grid">查看全部</el-button>
                  </div>
                </template>
                <div class="recent-projects-grid">
                  <div class="project-card" v-for="project in recentProjects" :key="project.id">
                    <div class="project-header">
                      <h4 class="project-name">{{ project.name }}</h4>
                      <el-tag :type="project.status === '已完成' ? 'success' : (project.status === '进行中' ? 'primary' : 'warning')">
                        {{ project.status }}
                      </el-tag>
                    </div>
                    <div class="project-info">
                      <div class="info-item">
                        <span class="label">当前阶段：</span>
                        <span class="value">{{ project.phase }}</span>
                      </div>
                      <div class="info-item">
                        <span class="label">负责人：</span>
                        <span class="value">{{ project.manager }}</span>
                      </div>
                      <div class="info-item">
                        <span class="label">截止日期：</span>
                        <span class="value">{{ project.deadline }}</span>
                      </div>
                    </div>
                    <div class="project-progress">
                      <div class="progress-label">进度：{{ project.progress }}%</div>
                      <el-progress :percentage="project.progress" :stroke-width="8" />
                    </div>
                    <div class="project-actions">
                      <el-button size="mini" type="primary" icon="el-icon-view">查看</el-button>
                      <el-button size="mini" type="success" icon="el-icon-edit">编辑</el-button>
                      <el-button size="mini" type="danger" icon="el-icon-delete">删除</el-button>
                    </div>
                  </div>
                </div>
              </el-card>
            </el-col>
          </el-row>
        </div>
        
        <!-- 进度管理页面 -->
        <div v-else-if="activeMenu === 'schedule'">
          <ScheduleManagement />
        </div>
        
        <!-- 统计分析页面 -->
        <div v-else-if="activeMenu === 'analysis'">
          <StatisticalAnalysis />
        </div>
        
        <!-- 用户管理页面 -->
        <div v-else-if="activeMenu === 'user'">
          <UserManagement />
        </div>
        
        <!-- 文档管理页面 -->
        <div v-else-if="activeMenu === 'document'">
          <DocumentManagement />
        </div>
        
        <!-- 远程监控页面 -->
        <div v-else-if="activeMenu === 'monitor'">
          <RemoteMonitoring />
        </div>
        
        <!-- 系统设置页面 -->
        <div v-else-if="activeMenu === 'settings'">
          <SystemSettings />
        </div>
        
        <!-- 其他页面内容 -->
        <div v-else class="page-content">
          <h2>{{ (menuItems.find(item => item.index === activeMenu) || {}).title || '未知' }}页面</h2>
          <p>内容待开发...</p>
        </div>
      </el-main>
    </div>
  </div>
</template>

<style>
/* 全局样式重置 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  width: 100%;
  height: 100%;
  overflow: hidden;
  margin: 0;
  padding: 0;
  background-color: var(--background-color);
}

/* 移除滚动条 */
::-webkit-scrollbar {
  display: none;
}

.app-container {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: var(--background-color);
  overflow: hidden;
}

.el-header {
  padding: 0 20px;
}

.header {
  background-color: var(--card-background);
  box-shadow: 0 2px 8px var(--shadow-color);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  z-index: 100;
  height: 60px;
  transition: all 0.3s ease;
}

.header:hover {
  box-shadow: 0 4px 12px var(--shadow-color);
}

.header-left {
  display: flex;
  align-items: center;
}

.system-title {
  font-size: 18px;
  font-weight: bold;
  color: var(--primary-color);
  margin-left: 20px;
  transition: color 0.3s ease;
}

.system-title:hover {
  color: var(--primary-light);
}

.header-right {
  display: flex;
  align-items: center;
}

.user-info {
  display: flex;
  align-items: center;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.user-info:hover {
  background-color: rgba(24, 144, 255, 0.1);
}

.user-info span {
  margin-left: 10px;
  color: var(--text-color);
}

.main-container {
  display: flex;
  flex: 1;
  overflow: hidden;
  width: 100%;
}

.aside {
  background-color: #001529;
  transition: width 0.3s ease;
  flex-shrink: 0;
  width: 200px;
}

.aside.is-collapse {
  width: 64px;
}

.el-menu-vertical-demo {
  height: 100%;
  border-right: none;
}

.main {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
  overflow-x: hidden;
  min-width: 0;
  background-color: var(--background-color);
}

/* 修复Element Plus布局样式 */
.el-aside {
  padding: 0;
}

.el-main {
  padding: 20px;
  flex: 1;
  min-width: 0;
  background-color: var(--background-color);
}

.dashboard {
  width: 100%;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  background-color: var(--card-background);
  border-bottom: 1px solid var(--border-color);
  border-radius: 8px 8px 0 0;
}

.card-content {
  text-align: center;
  padding: 30px 0;
  background-color: var(--card-background);
  border-radius: 0 0 8px 8px;
}

.number {
  font-size: 36px;
  font-weight: bold;
  color: var(--primary-color);
  transition: color 0.3s ease;
}

.number:hover {
  color: var(--primary-light);
}

.desc {
  font-size: 14px;
  color: var(--text-secondary);
  margin-top: 10px;
}

.chart-container {
  width: 100%;
  height: 100%;
  background-color: var(--card-background);
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 8px var(--shadow-color);
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.chart-container:hover {
  box-shadow: 0 4px 16px var(--shadow-color);
}

.page-content {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: var(--text-secondary);
}

/* 卡片样式优化 */
.el-card {
  border-radius: 8px;
  overflow: hidden;
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px var(--shadow-color);
}

.el-card:hover {
  box-shadow: 0 4px 16px var(--shadow-color);
  transform: translateY(-2px);
}

/* 按钮样式优化 */
.el-button {
  border-radius: 4px;
  transition: all 0.3s ease;
}

.el-button--primary {
  background-color: var(--primary-color);
  border-color: var(--primary-color);
}

.el-button--primary:hover {
  background-color: var(--primary-light);
  border-color: var(--primary-light);
}

/* 表格样式优化 */
.el-table {
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px var(--shadow-color);
  transition: all 0.3s ease;
}

.el-table:hover {
  box-shadow: 0 4px 16px var(--shadow-color);
}

/* 标签页样式优化 */
.el-tabs {
  background-color: var(--card-background);
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 8px var(--shadow-color);
  transition: all 0.3s ease;
}

.el-tabs:hover {
  box-shadow: 0 4px 16px var(--shadow-color);
}

/* 进度条样式优化 */
.el-progress__bar {
  border-radius: 10px;
}

.el-progress__bar__inner {
  background-color: var(--primary-color);
  transition: all 0.3s ease;
}

.el-progress__bar__inner:hover {
  background-color: var(--primary-light);
}

/* 标签样式优化 */
.el-tag {
  border-radius: 4px;
  transition: all 0.3s ease;
}

.el-tag:hover {
  transform: scale(1.05);
}

/* 输入框样式优化 */
.el-input {
  border-radius: 4px;
  transition: all 0.3s ease;
}

.el-input:focus-within {
  box-shadow: 0 0 0 2px rgba(24, 144, 255, 0.2);
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

/* 最近项目网格布局 */
.recent-projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 10px;
}

.project-card {
  background-color: #f9f9f9;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.project-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  transform: translateY(-2px);
}

.project-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.project-name {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin: 0;
}

.project-info {
  margin-bottom: 16px;
}

.info-item {
  display: flex;
  margin-bottom: 8px;
  font-size: 14px;
}

.info-item .label {
  font-weight: 500;
  color: #666;
  width: 80px;
  flex-shrink: 0;
}

.info-item .value {
  color: #333;
  flex: 1;
}

.project-progress {
  margin-bottom: 16px;
}

.progress-label {
  font-size: 14px;
  font-weight: 500;
  color: #333;
  margin-bottom: 8px;
}

.project-actions {
  display: flex;
  gap: 8px;
  justify-content: center;
}

.project-actions .el-button {
  min-width: 60px;
  font-size: 12px;
  padding: 4px 8px;
}

/* 响应式设计 */
@media screen and (max-width: 768px) {
  .recent-projects-grid {
    grid-template-columns: 1fr;
  }
  
  .project-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }
  
  .project-actions {
    flex-direction: column;
  }
  
  .project-actions .el-button {
    width: 100%;
  }
}
</style>
