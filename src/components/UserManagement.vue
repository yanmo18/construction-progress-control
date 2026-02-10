<script setup>
import { ref } from 'vue'

const userList = ref([
  {
    id: 1,
    username: 'admin',
    name: '管理员',
    role: '系统管理员',
    department: '总经办',
    phone: '13800138000',
    email: 'admin@example.com',
    status: '启用'
  },
  {
    id: 2,
    username: 'zhangsan',
    name: '张三',
    role: '项目经理',
    department: '工程部',
    phone: '13800138001',
    email: 'zhangsan@example.com',
    status: '启用'
  },
  {
    id: 3,
    username: 'lisi',
    name: '李四',
    role: '技术负责人',
    department: '技术部',
    phone: '13800138002',
    email: 'lisi@example.com',
    status: '启用'
  },
  {
    id: 4,
    username: 'wangwu',
    name: '王五',
    role: '安全员',
    department: '安全部',
    phone: '13800138003',
    email: 'wangwu@example.com',
    status: '禁用'
  }
])

const dialogVisible = ref(false)
const dialogTitle = ref('添加用户')
const currentUser = ref({})

const roles = ['系统管理员', '项目经理', '技术负责人', '安全员', '资料员', '施工员']
const departments = ['总经办', '工程部', '技术部', '安全部', '物资部', '财务部']

const openAddDialog = () => {
  dialogTitle.value = '添加用户'
  currentUser.value = {
    username: '',
    name: '',
    role: '',
    department: '',
    phone: '',
    email: '',
    status: '启用'
  }
  dialogVisible.value = true
}

const openEditDialog = (user) => {
  dialogTitle.value = '编辑用户'
  currentUser.value = { ...user }
  dialogVisible.value = true
}

const saveUser = () => {
  if (currentUser.value.id) {
    // 编辑用户
    const index = userList.value.findIndex(u => u.id === currentUser.value.id)
    if (index !== -1) {
      userList.value[index] = { ...currentUser.value }
    }
  } else {
    // 添加用户
    const newUser = {
      id: userList.value.length + 1,
      ...currentUser.value
    }
    userList.value.push(newUser)
  }
  dialogVisible.value = false
}

const deleteUser = (id) => {
  userList.value = userList.value.filter(u => u.id !== id)
}

const toggleStatus = (user) => {
  user.status = user.status === '启用' ? '禁用' : '启用'
}
</script>

<template>
  <div class="user-management">
    <el-card shadow="hover">
      <template #header>
        <div class="card-header">
          <span>用户管理</span>
          <el-button type="primary" icon="el-icon-plus" @click="openAddDialog">添加用户</el-button>
        </div>
      </template>
      
      <!-- 搜索和筛选 -->
      <div class="search-filter">
        <el-row :gutter="20">
          <el-col :span="6">
            <el-input placeholder="用户名/姓名" prefix-icon="el-icon-search"></el-input>
          </el-col>
          <el-col :span="6">
            <el-select placeholder="角色" style="width: 100%">
              <el-option label="全部" value=""></el-option>
              <el-option v-for="role in roles" :key="role" :label="role" :value="role"></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            <el-select placeholder="部门" style="width: 100%">
              <el-option label="全部" value=""></el-option>
              <el-option v-for="dept in departments" :key="dept" :label="dept" :value="dept"></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            <el-select placeholder="状态" style="width: 100%">
              <el-option label="全部" value=""></el-option>
              <el-option label="启用" value="启用"></el-option>
              <el-option label="禁用" value="禁用"></el-option>
            </el-select>
          </el-col>
        </el-row>
        <el-row :gutter="20" style="margin-top: 10px;">
          <el-col :span="6">
            <el-button type="primary" icon="el-icon-search">搜索</el-button>
            <el-button icon="el-icon-refresh">重置</el-button>
          </el-col>
        </el-row>
      </div>
      
      <!-- 用户列表 -->
      <div class="user-list">
        <el-table :data="userList" style="width: 100%" stripe>
          <el-table-column prop="id" label="ID" width="80"></el-table-column>
          <el-table-column prop="username" label="用户名" width="120"></el-table-column>
          <el-table-column prop="name" label="姓名" width="100"></el-table-column>
          <el-table-column prop="role" label="角色" width="120"></el-table-column>
          <el-table-column prop="department" label="部门" width="100"></el-table-column>
          <el-table-column prop="phone" label="电话" width="120"></el-table-column>
          <el-table-column prop="email" label="邮箱" width="150"></el-table-column>
          <el-table-column prop="status" label="状态" width="100">
            <template #default="scope">
              <el-tag :type="scope.row.status === '启用' ? 'success' : 'danger'">
                {{ scope.row.status }}
              </el-tag>
            </template>
          </el-table-column>
          <el-table-column label="操作" width="240" fixed="right">
            <template #default="scope">
              <div class="table-actions">
                <el-button size="mini" type="primary" icon="el-icon-view">查看</el-button>
                <el-button size="mini" type="success" icon="el-icon-edit" @click="openEditDialog(scope.row)">编辑</el-button>
                <el-button size="mini" type="warning" icon="el-icon-lock" @click="toggleStatus(scope.row)">{{ scope.row.status === '启用' ? '禁用' : '启用' }}</el-button>
                <el-button size="mini" type="danger" icon="el-icon-delete" @click="deleteUser(scope.row.id)">删除</el-button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
      
      <!-- 分页 -->
      <div class="pagination">
        <el-pagination
          layout="prev, pager, next, jumper, ->, total"
          :total="userList.length"
          :page-size="10"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        ></el-pagination>
      </div>
    </el-card>
    
    <!-- 添加/编辑用户对话框 -->
    <el-dialog v-model="dialogVisible" :title="dialogTitle" width="500px">
      <el-form :model="currentUser" label-width="80px">
        <el-form-item label="用户名">
          <el-input v-model="currentUser.username" placeholder="请输入用户名"></el-input>
        </el-form-item>
        <el-form-item label="姓名">
          <el-input v-model="currentUser.name" placeholder="请输入姓名"></el-input>
        </el-form-item>
        <el-form-item label="角色">
          <el-select v-model="currentUser.role" placeholder="请选择角色" style="width: 100%">
            <el-option v-for="role in roles" :key="role" :label="role" :value="role"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="部门">
          <el-select v-model="currentUser.department" placeholder="请选择部门" style="width: 100%">
            <el-option v-for="dept in departments" :key="dept" :label="dept" :value="dept"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="电话">
          <el-input v-model="currentUser.phone" placeholder="请输入电话"></el-input>
        </el-form-item>
        <el-form-item label="邮箱">
          <el-input v-model="currentUser.email" placeholder="请输入邮箱"></el-input>
        </el-form-item>
        <el-form-item label="状态">
          <el-select v-model="currentUser.status" style="width: 100%">
            <el-option label="启用" value="启用"></el-option>
            <el-option label="禁用" value="禁用"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="saveUser">确定</el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<style scoped>
.user-management {
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

.user-list {
  margin-bottom: 20px;
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