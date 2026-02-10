<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const activeTab = ref('video')
const videoList = ref([
  {
    id: 1,
    name: '工地入口',
    status: '在线',
    url: 'https://picsum.photos/id/1/600/400',
    lastUpdate: '2024-02-15 10:30:00'
  },
  {
    id: 2,
    name: '主体结构',
    status: '在线',
    url: 'https://picsum.photos/id/2/600/400',
    lastUpdate: '2024-02-15 10:29:00'
  },
  {
    id: 3,
    name: '材料堆场',
    status: '离线',
    url: 'https://picsum.photos/id/3/600/400',
    lastUpdate: '2024-02-15 09:15:00'
  },
  {
    id: 4,
    name: '塔吊',
    status: '在线',
    url: 'https://picsum.photos/id/4/600/400',
    lastUpdate: '2024-02-15 10:28:00'
  }
])

const sensorData = ref([
  {
    id: 1,
    name: '温度传感器',
    value: 25.5,
    unit: '℃',
    status: '正常',
    location: '工地入口'
  },
  {
    id: 2,
    name: '湿度传感器',
    value: 65,
    unit: '%',
    status: '正常',
    location: '主体结构'
  },
  {
    id: 3,
    name: '噪声传感器',
    value: 78,
    unit: 'dB',
    status: '异常',
    location: '主体结构'
  },
  {
    id: 4,
    name: '扬尘传感器',
    value: 23,
    unit: 'μg/m³',
    status: '正常',
    location: '材料堆场'
  }
])

const alarmList = ref([
  {
    id: 1,
    type: '噪声超标',
    level: '警告',
    message: '主体结构区域噪声超过75dB',
    time: '2024-02-15 10:15:30',
    status: '未处理'
  },
  {
    id: 2,
    type: '设备离线',
    level: '严重',
    message: '材料堆场摄像头离线',
    time: '2024-02-15 09:15:00',
    status: '已处理'
  },
  {
    id: 3,
    type: '温度异常',
    level: '一般',
    message: '工地入口温度超过30℃',
    time: '2024-02-15 08:30:00',
    status: '已处理'
  }
])

const selectedCamera = ref(videoList.value[0])
const updateInterval = ref(null)

const selectCamera = (camera) => {
  selectedCamera.value = camera
}

const handleAlarm = (alarm) => {
  alarm.status = '已处理'
}

const startRealTimeUpdate = () => {
  updateInterval.value = setInterval(() => {
    // 模拟传感器数据更新
    sensorData.value.forEach(sensor => {
      if (sensor.status === '正常') {
        sensor.value += (Math.random() - 0.5) * 2
        sensor.value = parseFloat(sensor.value.toFixed(1))
      }
      
      // 模拟异常情况
      if (Math.random() > 0.95) {
        sensor.status = sensor.status === '正常' ? '异常' : '正常'
      }
    })
    
    // 模拟告警生成
    if (Math.random() > 0.97) {
      const newAlarm = {
        id: alarmList.value.length + 1,
        type: ['温度异常', '湿度异常', '噪声超标', '扬尘超标', '设备离线'][Math.floor(Math.random() * 5)],
        level: ['一般', '警告', '严重'][Math.floor(Math.random() * 3)],
        message: '模拟告警信息',
        time: new Date().toLocaleString('zh-CN'),
        status: '未处理'
      }
      alarmList.value.unshift(newAlarm)
    }
  }, 5000)
}

const stopRealTimeUpdate = () => {
  if (updateInterval.value) {
    clearInterval(updateInterval.value)
    updateInterval.value = null
  }
}

onMounted(() => {
  startRealTimeUpdate()
})

onUnmounted(() => {
  stopRealTimeUpdate()
})
</script>

<template>
  <div class="remote-monitoring">
    <el-card shadow="hover">
      <template #header>
        <div class="card-header">
          <span>远程监控</span>
          <el-button type="primary" icon="el-icon-video-camera">查看全部</el-button>
        </div>
      </template>
      
      <!-- 标签页 -->
      <el-tabs v-model="activeTab" type="card">
        <!-- 视频监控 -->
        <el-tab-pane label="视频监控" name="video">
          <div class="video-monitoring">
            <div class="camera-list">
              <el-row :gutter="20">
                <el-col :span="6" v-for="camera in videoList" :key="camera.id" class="camera-item">
                  <el-card :class="{ 'active': selectedCamera.id === camera.id }" @click="selectCamera(camera)">
                    <template #header>
                      <div class="camera-header">
                        <span>{{ camera.name }}</span>
                        <el-tag :type="camera.status === '在线' ? 'success' : 'danger'" size="small">
                          {{ camera.status }}
                        </el-tag>
                      </div>
                    </template>
                    <div class="camera-preview">
                      <img :src="camera.url" alt="{{ camera.name }}" class="camera-image">
                    </div>
                    <div class="camera-footer">
                      <span>{{ camera.lastUpdate }}</span>
                    </div>
                  </el-card>
                </el-col>
              </el-row>
            </div>
            
            <div class="main-camera">
              <el-card shadow="hover" v-if="selectedCamera">
                <template #header>
                  <div class="camera-header">
                    <span>{{ selectedCamera.name }}</span>
                    <el-tag :type="selectedCamera.status === '在线' ? 'success' : 'danger'">
                      {{ selectedCamera.status }}
                    </el-tag>
                  </div>
                </template>
                <div class="main-preview">
                  <img :src="selectedCamera.url" alt="{{ selectedCamera.name }}" class="main-image">
                </div>
                <div class="camera-controls">
                  <el-button type="primary" icon="el-icon-video-play">开始录制</el-button>
                  <el-button icon="el-icon-camera">截图</el-button>
                  <el-button icon="el-icon-full-screen">全屏</el-button>
                  <el-button icon="el-icon-refresh">刷新</el-button>
                </div>
              </el-card>
            </div>
          </div>
        </el-tab-pane>
        
        <!-- 实时数据 -->
        <el-tab-pane label="实时数据" name="data">
          <div class="real-time-data">
            <el-row :gutter="20">
              <el-col :span="6" v-for="sensor in sensorData" :key="sensor.id">
                <el-card shadow="hover" :class="{ 'warning': sensor.status === '异常' }">
                  <template #header>
                    <div class="sensor-header">
                      <span>{{ sensor.name }}</span>
                      <el-tag :type="sensor.status === '正常' ? 'success' : 'danger'">
                        {{ sensor.status }}
                      </el-tag>
                    </div>
                  </template>
                  <div class="sensor-value">
                    <span class="value">{{ sensor.value }}</span>
                    <span class="unit">{{ sensor.unit }}</span>
                  </div>
                  <div class="sensor-footer">
                    <span>{{ sensor.location }}</span>
                  </div>
                </el-card>
              </el-col>
            </el-row>
          </div>
        </el-tab-pane>
        
        <!-- 告警信息 -->
        <el-tab-pane label="告警信息" name="alarm">
          <div class="alarm-list">
            <el-table :data="alarmList" style="width: 100%" stripe>
              <el-table-column prop="id" label="ID" width="80"></el-table-column>
              <el-table-column prop="type" label="告警类型" width="120"></el-table-column>
              <el-table-column prop="level" label="严重程度" width="100">
                <template #default="scope">
                  <el-tag :type="scope.row.level === '严重' ? 'danger' : scope.row.level === '警告' ? 'warning' : 'info'">
                    {{ scope.row.level }}
                  </el-tag>
                </template>
              </el-table-column>
              <el-table-column prop="message" label="告警信息" min-width="200"></el-table-column>
              <el-table-column prop="time" label="发生时间" width="150"></el-table-column>
              <el-table-column prop="status" label="处理状态" width="100">
                <template #default="scope">
                  <el-tag :type="scope.row.status === '已处理' ? 'success' : 'warning'">
                    {{ scope.row.status }}
                  </el-tag>
                </template>
              </el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template #default="scope">
                  <el-button size="small" type="primary" icon="el-icon-check" @click="handleAlarm(scope.row)" v-if="scope.row.status === '未处理'">处理</el-button>
                  <el-button size="small" type="info" icon="el-icon-view">详情</el-button>
                </template>
              </el-table-column>
            </el-table>
          </div>
        </el-tab-pane>
      </el-tabs>
    </el-card>
  </div>
</template>

<style scoped>
.remote-monitoring {
  width: 100%;
  height: 100%;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* 视频监控样式 */
.video-monitoring {
  width: 100%;
}

.camera-list {
  margin-bottom: 20px;
}

.camera-item {
  cursor: pointer;
}

.camera-item .el-card {
  transition: all 0.3s;
}

.camera-item .el-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

.camera-item .el-card.active {
  border: 2px solid #1890ff;
}

.camera-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.camera-preview {
  height: 150px;
  overflow: hidden;
  margin: 10px 0;
}

.camera-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.camera-footer {
  font-size: 12px;
  color: #666;
  text-align: right;
}

.main-camera {
  margin-top: 20px;
}

.main-preview {
  height: 400px;
  overflow: hidden;
  margin: 10px 0;
}

.main-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.camera-controls {
  display: flex;
  justify-content: flex-start;
  gap: 10px;
  margin-top: 10px;
}

/* 实时数据样式 */
.real-time-data {
  width: 100%;
}

.sensor-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.sensor-value {
  text-align: center;
  margin: 20px 0;
}

.value {
  font-size: 36px;
  font-weight: bold;
  color: #1890ff;
}

.unit {
  font-size: 18px;
  color: #666;
  margin-left: 5px;
}

.sensor-footer {
  font-size: 14px;
  color: #666;
  text-align: right;
}

.el-card.warning {
  border-left: 4px solid #f56c6c;
}

/* 告警信息样式 */
.alarm-list {
  width: 100%;
}
</style>