<template>
  <div class="statistical-analysis">
    <el-card shadow="hover">
      <template #header>
        <div class="card-header">
          <span>统计分析</span>
          <el-button type="primary" icon="el-icon-download">导出报表</el-button>
        </div>
      </template>
      
      <el-tabs v-model="activeTab" type="card">
        <el-tab-pane label="项目进度" name="progress">
          <div class="chart-container" style="height: 400px;">
            <div id="analysisProgressChart" style="width: 100%; height: 100%;"></div>
          </div>
        </el-tab-pane>
        <el-tab-pane label="资源使用" name="resource">
          <div class="chart-container" style="height: 400px;">
            <div id="analysisResourceChart" style="width: 100%; height: 100%;"></div>
          </div>
        </el-tab-pane>
        <el-tab-pane label="成本分析" name="cost">
          <div class="chart-container" style="height: 400px;">
            <div id="analysisCostChart" style="width: 100%; height: 100%;"></div>
          </div>
        </el-tab-pane>
        <el-tab-pane label="质量安全" name="quality">
          <div class="chart-container" style="height: 400px;">
            <div id="analysisQualityChart" style="width: 100%; height: 100%;"></div>
          </div>
        </el-tab-pane>
      </el-tabs>
      
      <div class="stat-cards">
        <el-row :gutter="20" style="margin-top: 20px;">
          <el-col :span="6">
            <el-card shadow="hover">
              <div class="card-content">
                <div class="number">¥2,450,000</div>
                <div class="desc">总投资</div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="6">
            <el-card shadow="hover">
              <div class="card-content">
                <div class="number">¥1,280,000</div>
                <div class="desc">已完成投资</div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="6">
            <el-card shadow="hover">
              <div class="card-content">
                <div class="number">85%</div>
                <div class="desc">平均进度</div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="6">
            <el-card shadow="hover">
              <div class="card-content">
                <div class="number">12</div>
                <div class="desc">安全隐患</div>
              </div>
            </el-card>
          </el-col>
        </el-row>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 'progress'
    }
  }
}
</script>

<style scoped>
.statistical-analysis {
  width: 100%;
  height: 100%;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chart-container {
  width: 100%;
  height: 400px;
  margin-top: 20px;
}

.stat-cards {
  margin-top: 20px;
}

.card-content {
  text-align: center;
  padding: 20px 0;
}

.number {
  font-size: 24px;
  font-weight: bold;
  color: #1890ff;
}

.desc {
  font-size: 14px;
  color: #666;
  margin-top: 10px;
}
</style>