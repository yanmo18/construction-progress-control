<script setup>
import { ref } from 'vue'
import { ElMessage } from 'element-plus'
import { Plus } from '@element-plus/icons-vue'

const activeTab = ref('basic')

// 基本设置
const basicSettings = ref({
  systemName: '建筑施工进度数字化管控系统',
  companyName: '某某建筑工程有限公司',
  contactPerson: '管理员',
  contactPhone: '13800138000',
  email: 'admin@example.com',
  logo: ''
})

// 通知设置
const notificationSettings = ref({
  emailNotification: true,
  smsNotification: false,
  systemNotification: true,
  alarmNotification: true,
  emailServer: 'smtp.example.com',
  emailPort: '465',
  emailAccount: 'notification@example.com',
  emailPassword: '********'
})

// 数据设置
const dataSettings = ref({
  backupInterval: '7', // 天
  backupPath: 'D:\\backup\\',
  maxBackupCount: '10',
  autoCleanup: true,
  cleanupDays: '30',
  databaseType: 'MongoDB',
  databaseHost: 'localhost',
  databasePort: '27017',
  databaseName: 'construction_management'
})

// 权限设置
const permissionSettings = ref({
  roles: [
    {
      id: 1,
      name: '系统管理员',
      permissions: ['全部权限'],
      description: '拥有系统所有权限'
    },
    {
      id: 2,
      name: '项目经理',
      permissions: ['项目管理', '进度管理', '资源管理', '质量安全', '文档管理'],
      description: '管理项目相关所有操作'
    },
    {
      id: 3,
      name: '技术负责人',
      permissions: ['技术管理', '文档管理', '质量安全'],
      description: '负责技术相关工作'
    },
    {
      id: 4,
      name: '安全员',
      permissions: ['质量安全', '远程监控'],
      description: '负责安全检查和监控'
    },
    {
      id: 5,
      name: '资料员',
      permissions: ['文档管理'],
      description: '负责文档整理和管理'
    }
  ]
})

const saveSettings = (tab) => {
  console.log(`保存${tab}设置:`, tab === 'basic' ? basicSettings.value : tab === 'notification' ? notificationSettings.value : tab === 'data' ? dataSettings.value : permissionSettings.value)
  
  // 模拟保存成功
  ElMessage.success('设置保存成功')
}

const resetSettings = (tab) => {
  if (tab === 'basic') {
    basicSettings.value = {
      systemName: '建筑施工进度数字化管控系统',
      companyName: '某某建筑工程有限公司',
      contactPerson: '管理员',
      contactPhone: '13800138000',
      email: 'admin@example.com',
      logo: ''
    }
  } else if (tab === 'notification') {
    notificationSettings.value = {
      emailNotification: true,
      smsNotification: false,
      systemNotification: true,
      alarmNotification: true,
      emailServer: 'smtp.example.com',
      emailPort: '465',
      emailAccount: 'notification@example.com',
      emailPassword: '********'
    }
  } else if (tab === 'data') {
    dataSettings.value = {
      backupInterval: '7',
      backupPath: 'D:\\backup\\',
      maxBackupCount: '10',
      autoCleanup: true,
      cleanupDays: '30',
      databaseType: 'MongoDB',
      databaseHost: 'localhost',
      databasePort: '27017',
      databaseName: 'construction_management'
    }
  }
  
  ElMessage.info('设置已重置')
}

const testEmail = () => {
  console.log('测试邮件发送')
  ElMessage.success('测试邮件已发送，请查收')
}

const performBackup = () => {
  console.log('执行手动备份')
  ElMessage.success('备份执行成功')
}

const addRole = () => {
  const newRole = {
    id: permissionSettings.value.roles.length + 1,
    name: '新角色',
    permissions: [],
    description: ''
  }
  permissionSettings.value.roles.push(newRole)
}

const deleteRole = (id) => {
  permissionSettings.value.roles = permissionSettings.value.roles.filter(role => role.id !== id)
}

const handleAvatarSuccess = (response, uploadFile) => {
  basicSettings.value.logo = URL.createObjectURL(uploadFile.raw)
}

const beforeAvatarUpload = (file) => {
  const isJPG = file.type === 'image/jpeg' || file.type === 'image/png'
  const isLt2M = file.size / 1024 / 1024 < 2

  if (!isJPG) {
    ElMessage.error('只能上传JPG/PNG图片!')
  }
  if (!isLt2M) {
    ElMessage.error('图片大小不能超过2MB!')
  }
  return isJPG && isLt2M
}
</script>

<template>
  <div class="system-settings">
    <el-card shadow="hover">
      <template #header>
        <div class="card-header">
          <span>系统设置</span>
          <el-button type="primary" icon="el-icon-save" @click="saveSettings(activeTab)">保存设置</el-button>
        </div>
      </template>
      
      <!-- 标签页 -->
      <el-tabs v-model="activeTab" type="card">
        <!-- 基本设置 -->
        <el-tab-pane label="基本设置" name="basic">
          <el-form :model="basicSettings" label-width="120px">
            <el-form-item label="系统名称">
              <el-input v-model="basicSettings.systemName" placeholder="请输入系统名称"></el-input>
            </el-form-item>
            <el-form-item label="公司名称">
              <el-input v-model="basicSettings.companyName" placeholder="请输入公司名称"></el-input>
            </el-form-item>
            <el-form-item label="联系人">
              <el-input v-model="basicSettings.contactPerson" placeholder="请输入联系人"></el-input>
            </el-form-item>
            <el-form-item label="联系电话">
              <el-input v-model="basicSettings.contactPhone" placeholder="请输入联系电话"></el-input>
            </el-form-item>
            <el-form-item label="电子邮箱">
              <el-input v-model="basicSettings.email" placeholder="请输入电子邮箱"></el-input>
            </el-form-item>
            <el-form-item label="系统Logo">
              <el-upload
                class="avatar-uploader"
                action="#"
                :show-file-list="false"
                :on-success="handleAvatarSuccess"
                :before-upload="beforeAvatarUpload"
              >
                <img v-if="basicSettings.logo" :src="basicSettings.logo" class="avatar">
                <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
              </el-upload>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="saveSettings('basic')">保存设置</el-button>
              <el-button @click="resetSettings('basic')">重置</el-button>
            </el-form-item>
          </el-form>
        </el-tab-pane>
        
        <!-- 通知设置 -->
        <el-tab-pane label="通知设置" name="notification">
          <el-form :model="notificationSettings" label-width="120px">
            <el-form-item label="通知方式">
              <el-checkbox v-model="notificationSettings.emailNotification">邮件通知</el-checkbox>
              <el-checkbox v-model="notificationSettings.smsNotification">短信通知</el-checkbox>
              <el-checkbox v-model="notificationSettings.systemNotification">系统通知</el-checkbox>
              <el-checkbox v-model="notificationSettings.alarmNotification">告警通知</el-checkbox>
            </el-form-item>
            <el-form-item label="邮件服务器">
              <el-input v-model="notificationSettings.emailServer" placeholder="请输入邮件服务器地址"></el-input>
            </el-form-item>
            <el-form-item label="邮件端口">
              <el-input v-model="notificationSettings.emailPort" placeholder="请输入邮件服务器端口"></el-input>
            </el-form-item>
            <el-form-item label="邮件账号">
              <el-input v-model="notificationSettings.emailAccount" placeholder="请输入邮件账号"></el-input>
            </el-form-item>
            <el-form-item label="邮件密码">
              <el-input v-model="notificationSettings.emailPassword" type="password" placeholder="请输入邮件密码"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="saveSettings('notification')">保存设置</el-button>
              <el-button @click="resetSettings('notification')">重置</el-button>
              <el-button type="success" @click="testEmail">测试邮件</el-button>
            </el-form-item>
          </el-form>
        </el-tab-pane>
        
        <!-- 数据设置 -->
        <el-tab-pane label="数据设置" name="data">
          <el-form :model="dataSettings" label-width="120px">
            <el-form-item label="备份间隔">
              <el-input v-model="dataSettings.backupInterval" style="width: 100px"></el-input>
              <span style="margin-left: 10px">天</span>
            </el-form-item>
            <el-form-item label="备份路径">
              <el-input v-model="dataSettings.backupPath" placeholder="请输入备份路径"></el-input>
            </el-form-item>
            <el-form-item label="最大备份数">
              <el-input v-model="dataSettings.maxBackupCount" style="width: 100px"></el-input>
              <span style="margin-left: 10px">个</span>
            </el-form-item>
            <el-form-item label="自动清理">
              <el-switch v-model="dataSettings.autoCleanup"></el-switch>
            </el-form-item>
            <el-form-item label="清理天数">
              <el-input v-model="dataSettings.cleanupDays" style="width: 100px"></el-input>
              <span style="margin-left: 10px">天</span>
            </el-form-item>
            <el-form-item label="数据库类型">
              <el-select v-model="dataSettings.databaseType" style="width: 200px">
                <el-option label="MongoDB" value="MongoDB"></el-option>
                <el-option label="MySQL" value="MySQL"></el-option>
                <el-option label="SQL Server" value="SQL Server"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="数据库主机">
              <el-input v-model="dataSettings.databaseHost" placeholder="请输入数据库主机地址"></el-input>
            </el-form-item>
            <el-form-item label="数据库端口">
              <el-input v-model="dataSettings.databasePort" placeholder="请输入数据库端口"></el-input>
            </el-form-item>
            <el-form-item label="数据库名称">
              <el-input v-model="dataSettings.databaseName" placeholder="请输入数据库名称"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="saveSettings('data')">保存设置</el-button>
              <el-button @click="resetSettings('data')">重置</el-button>
              <el-button type="success" @click="performBackup">手动备份</el-button>
            </el-form-item>
          </el-form>
        </el-tab-pane>
        
        <!-- 权限设置 -->
        <el-tab-pane label="权限设置" name="permission">
          <div class="permission-settings">
            <el-button type="primary" icon="el-icon-plus" @click="addRole" style="margin-bottom: 20px">添加角色</el-button>
            
            <el-table :data="permissionSettings.roles" style="width: 100%" stripe>
              <el-table-column prop="id" label="ID" width="80"></el-table-column>
              <el-table-column prop="name" label="角色名称" width="120"></el-table-column>
              <el-table-column prop="permissions" label="权限" min-width="200">
                <template #default="scope">
                  <el-tag v-for="(perm, index) in scope.row.permissions" :key="index" size="small" style="margin-right: 5px; margin-bottom: 5px">
                    {{ perm }}
                  </el-tag>
                </template>
              </el-table-column>
              <el-table-column prop="description" label="描述" min-width="150"></el-table-column>
              <el-table-column label="操作" width="150" fixed="right">
                <template #default="scope">
                  <el-button size="small" type="primary" icon="el-icon-edit">编辑</el-button>
                  <el-button size="small" type="danger" icon="el-icon-delete" @click="deleteRole(scope.row.id)">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </div>
          <el-form-item style="margin-top: 20px">
            <el-button type="primary" @click="saveSettings('permission')">保存设置</el-button>
          </el-form-item>
        </el-tab-pane>
      </el-tabs>
    </el-card>
  </div>
</template>

<style scoped>
.system-settings {
  width: 100%;
  height: 100%;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.avatar-uploader {
  width: 120px;
  height: 120px;
}

.avatar {
  width: 120px;
  height: 120px;
  object-fit: cover;
}

.permission-settings {
  width: 100%;
}
</style>