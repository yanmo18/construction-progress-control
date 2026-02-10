<script setup>
import { ref } from 'vue'

const documentList = ref([
  {
    id: 1,
    name: '项目A施工组织设计方案.docx',
    category: '施工方案',
    project: '项目A',
    uploader: '张三',
    uploadTime: '2024-01-10 14:30',
    size: '2.5MB',
    version: 'v1.0',
    downloads: 15,
    status: '已发布'
  },
  {
    id: 2,
    name: '项目A基础工程验收报告.pdf',
    category: '验收报告',
    project: '项目A',
    uploader: '李四',
    uploadTime: '2024-02-15 09:15',
    size: '1.8MB',
    version: 'v1.0',
    downloads: 8,
    status: '已发布'
  },
  {
    id: 3,
    name: '项目B主体结构施工图纸.dwg',
    category: '施工图纸',
    project: '项目B',
    uploader: '王五',
    uploadTime: '2024-01-20 16:45',
    size: '15.2MB',
    version: 'v2.0',
    downloads: 23,
    status: '已发布'
  },
  {
    id: 4,
    name: '项目C安全技术交底记录.docx',
    category: '安全资料',
    project: '项目C',
    uploader: '赵六',
    uploadTime: '2024-02-05 11:20',
    size: '0.8MB',
    version: 'v1.0',
    downloads: 5,
    status: '草稿'
  }
])

const dialogVisible = ref(false)
const dialogTitle = ref('上传文档')
const currentDocument = ref({})
const fileList = ref([])

const categories = ['施工方案', '验收报告', '施工图纸', '安全资料', '物资管理', '财务资料']
const projects = ['项目A', '项目B', '项目C', '项目D', '项目E']

const openUploadDialog = () => {
  dialogTitle.value = '上传文档'
  currentDocument.value = {
    name: '',
    category: '',
    project: '',
    version: 'v1.0',
    status: '已发布'
  }
  dialogVisible.value = true
}

const downloadDocument = (doc) => {
  console.log('下载文档:', doc.name)
}

const previewDocument = (doc) => {
  console.log('预览文档:', doc.name)
}

const deleteDocument = (id) => {
  documentList.value = documentList.value.filter(doc => doc.id !== id)
}

const uploadDocument = () => {
  const newDoc = {
    id: documentList.value.length + 1,
    ...currentDocument.value,
    uploader: '管理员',
    uploadTime: new Date().toLocaleString('zh-CN'),
    size: '1.0MB',
    downloads: 0
  }
  documentList.value.push(newDoc)
  dialogVisible.value = false
}

const handlePreview = (file) => {
  console.log('预览文件:', file.name)
}

const handleRemove = (file, fileList) => {
  console.log('移除文件:', file.name)
}
</script>

<template>
  <div class="document-management">
    <el-card shadow="hover">
      <template #header>
        <div class="card-header">
          <span>文档管理</span>
          <el-button type="primary" icon="el-icon-upload" @click="openUploadDialog">上传文档</el-button>
        </div>
      </template>
      
      <!-- 搜索和筛选 -->
      <div class="search-filter">
        <el-row :gutter="20">
          <el-col :span="6">
            <el-input placeholder="文档名称" prefix-icon="el-icon-search"></el-input>
          </el-col>
          <el-col :span="6">
            <el-select placeholder="分类" style="width: 100%">
              <el-option label="全部" value=""></el-option>
              <el-option v-for="category in categories" :key="category" :label="category" :value="category"></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            <el-select placeholder="项目" style="width: 100%">
              <el-option label="全部" value=""></el-option>
              <el-option v-for="project in projects" :key="project" :label="project" :value="project"></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            <el-date-picker
              type="daterange"
              range-separator="至"
              start-placeholder="上传开始日期"
              end-placeholder="上传结束日期"
              style="width: 100%"
            ></el-date-picker>
          </el-col>
        </el-row>
        <el-row :gutter="20" style="margin-top: 10px;">
          <el-col :span="6">
            <el-button type="primary" icon="el-icon-search">搜索</el-button>
            <el-button icon="el-icon-refresh">重置</el-button>
          </el-col>
        </el-row>
      </div>
      
      <!-- 文档列表 -->
      <div class="document-list">
        <el-table :data="documentList" style="width: 100%" stripe>
          <el-table-column prop="id" label="ID" width="80"></el-table-column>
          <el-table-column prop="name" label="文档名称" min-width="200">
            <template #default="scope">
              <span class="document-name">{{ scope.row.name }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="category" label="分类" width="120"></el-table-column>
          <el-table-column prop="project" label="项目" width="100"></el-table-column>
          <el-table-column prop="uploader" label="上传人" width="100"></el-table-column>
          <el-table-column prop="uploadTime" label="上传时间" width="150"></el-table-column>
          <el-table-column prop="size" label="大小" width="80"></el-table-column>
          <el-table-column prop="version" label="版本" width="80"></el-table-column>
          <el-table-column prop="downloads" label="下载次数" width="100"></el-table-column>
          <el-table-column prop="status" label="状态" width="100">
            <template #default="scope">
              <el-tag :type="scope.row.status === '已发布' ? 'success' : 'warning'">
                {{ scope.row.status }}
              </el-tag>
            </template>
          </el-table-column>
          <el-table-column label="操作" width="180" fixed="right">
            <template #default="scope">
              <div class="table-actions">
                <el-button size="mini" type="primary" icon="el-icon-view" @click="previewDocument(scope.row)">预览</el-button>
                <el-button size="mini" type="success" icon="el-icon-download" @click="downloadDocument(scope.row)">下载</el-button>
                <el-button size="mini" type="danger" icon="el-icon-delete" @click="deleteDocument(scope.row.id)">删除</el-button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
      
      <!-- 分页 -->
      <div class="pagination">
        <el-pagination
          layout="prev, pager, next, jumper, ->, total"
          :total="documentList.length"
          :page-size="10"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        ></el-pagination>
      </div>
    </el-card>
    
    <!-- 上传文档对话框 -->
    <el-dialog v-model="dialogVisible" :title="dialogTitle" width="500px">
      <el-form :model="currentDocument" label-width="80px">
        <el-form-item label="文档名称">
          <el-input v-model="currentDocument.name" placeholder="请输入文档名称"></el-input>
        </el-form-item>
        <el-form-item label="文档分类">
          <el-select v-model="currentDocument.category" placeholder="请选择分类" style="width: 100%">
            <el-option v-for="category in categories" :key="category" :label="category" :value="category"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="所属项目">
          <el-select v-model="currentDocument.project" placeholder="请选择项目" style="width: 100%">
            <el-option v-for="project in projects" :key="project" :label="project" :value="project"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="版本号">
          <el-input v-model="currentDocument.version" placeholder="请输入版本号"></el-input>
        </el-form-item>
        <el-form-item label="状态">
          <el-select v-model="currentDocument.status" style="width: 100%">
            <el-option label="已发布" value="已发布"></el-option>
            <el-option label="草稿" value="草稿"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="上传文件">
          <el-upload
            class="upload-demo"
            action="#"
            :on-preview="handlePreview"
            :on-remove="handleRemove"
            :file-list="fileList"
            :auto-upload="false"
          >
            <el-button type="primary">点击上传</el-button>
            <template #tip>
              <div class="el-upload__tip">
                只能上传 doc/docx/pdf/dwg 文件，且不超过 50MB
              </div>
            </template>
          </el-upload>
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="uploadDocument">确定上传</el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<style scoped>
.document-management {
  width: 100%;
  height: 100%;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.search-filter {
  margin-bottom: 20px;
}

.document-list {
  margin-bottom: 20px;
}

.document-name {
  font-weight: 500;
  color: #1890ff;
  cursor: pointer;
}

.document-name:hover {
  text-decoration: underline;
}

.pagination {
  display: flex;
  justify-content: flex-end;
  margin-top: 20px;
}

.dialog-footer {
  width: 100%;
  display: flex;
  justify-content: flex-end;
}

.upload-demo {
  width: 100%;
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
</style>