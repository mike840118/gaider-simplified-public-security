<template>
  <div v-if="isOpen" class="modal-overlay sub-modal-overlay" @click.self="closeModal">
    <div class="modal-content user-select-modal">
      <div class="modal-header">
        <h2 class="modal-title">{{ $t('user_manage.select_user', '選擇使用者') }}</h2>
        <button class="close-btn" @click="closeModal">✕</button>
      </div>

      <div class="search-bar">
        <input type="text" v-model="searchName" :placeholder="$t('common.search_user_placeholder', '使用者姓名')"
          class="search-input" />
        <input type="text" v-model="searchAccount" :placeholder="$t('common.account_placeholder', '帳號')"
          class="search-input" />
        <button class="btn-search">{{ $t('common.search', '搜尋') }}</button>
      </div>

      <div class="layout-body">
        <div class="sidebar">
          <div class="sidebar-header">
            <span>{{ $t('user_manage.all_org', '全部機構') }}</span>
            <span class="icon">📖</span>
          </div>
          <ul class="tree-menu">
            <li class="tree-item expanded">
              <span class="arrow">▼</span> 蓋德健康
              <ul class="sub-menu">
                <li><span class="arrow">▶</span> 台灣健康管理中心</li>
                <li class="expanded">
                  <span class="arrow">▼</span> Guider Singapore Branch
                  <ul class="sub-menu inner">
                    <li class="active">蓋德</li>
                    <li>Luke's Hospital</li>
                    <li>XN Engineering Pte Ltd</li>
                    <li>Country Park Condominium</li>
                    <li>health way</li>
                  </ul>
                </li>
              </ul>
            </li>
          </ul>
        </div>

        <div class="main-content">
          <div class="table-container">
            <table class="user-table">
              <thead>
                <tr>
                  <th>{{ $t('data_manage.table.avatar', '大頭貼') }}</th>
                  <th>{{ $t('data_manage.table.name', '姓名') }}</th>
                  <th>{{ $t('data_manage.table.gender', '性別') }}</th>
                  <th>{{ $t('user_manage.table.account', '帳號(ID)') }}</th>
                  <th>{{ $t('user_manage.table.org', '所屬機構') }}</th>
                  <th>{{ $t('common.age', '年齡') }}</th>
                  <th>{{ $t('data_manage.table.action', '操作') }}</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="user in filteredPatientList" :key="user.id">
                  <td>
                    <img v-if="user.avatar" :src="user.avatar" class="user-avatar" alt="avatar" />
                    <div v-else class="avatar-placeholder">👤</div>
                  </td>
                  <td>{{ user.name }}</td>
                  <td>{{ user.gender }}</td>
                  <td>{{ user.account }}</td>
                  <td>{{ user.org }}</td>
                  <td>{{ user.age }}</td>
                  <td>
                    <button class="btn-text-select" @click="selectUser(user)">
                      {{ $t('common.select', '選擇') }}
                    </button>
                  </td>
                </tr>
                <tr v-if="filteredPatientList.length === 0">
                  <td colspan="7" style="text-align: center; color: #999; padding: 24px;">
                    {{ $t('user_manage.no_match', '找不到符合名稱的用戶') }}
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import rawPatientData from '@/mock/patients.json' // 確保這個路徑符合你專案的檔案結構

const props = defineProps({ isOpen: Boolean })
const emit = defineEmits(['close', 'select'])

// 搜尋關鍵字綁定
const searchName = ref('')
const searchAccount = ref('')

// 計算年齡 (以當前設定為 2026 年為基準)
const calculateAge = (birthday) => {
  if (!birthday) return '未知'
  const birthYear = new Date(birthday).getFullYear()
  const currentYear = 2026
  return currentYear - birthYear
}

// 轉換性別字串
const formatGender = (genderCode) => {
  if (genderCode === 'MAN') return '男'
  if (genderCode === 'WOMAN') return '女'
  return '未知'
}

// 1. 基本的資料清洗格式化
const patientList = computed(() => {
  const data = rawPatientData.data?.data || []
  return data.map(p => ({
    id: p.accountId,
    name: p.name || '未知使用者',
    avatar: p.url || '',
    gender: formatGender(p.gender),
    account: String(p.accountId), // 轉成字串方便進行 String.includes() 模糊比對
    org: p.companyName || '未知機構',
    age: calculateAge(p.birthday)
  }))
})

// 2. 核心：即時動態模糊搜尋邏輯
const filteredPatientList = computed(() => {
  return patientList.value.filter(user => {
    // 姓名模糊比對 (忽略大小寫)
    const matchName = user.name.toLowerCase().includes(searchName.value.toLowerCase().trim())

    // 帳號/ID 模糊比對
    const matchAccount = user.account.includes(searchAccount.value.trim())

    // 兩者條件必須同時符合（如果其中一個沒輸入，則該條件默認為 true）
    return matchName && matchAccount
  })
})

const closeModal = () => {
  emit('close')
  // 彈窗關閉後清空搜尋條件，下次打開時能看到完整的清單
  setTimeout(() => {
    searchName.value = ''
    searchAccount.value = ''
  }, 300)
}

const selectUser = (user) => {
  emit('select', user) // 將選中的使用者資料傳回父元件
}
</script>
<style scoped>
/* 增加 z-index 確保蓋在原本的彈窗上方 */
.modal-overlay.sub-modal-overlay {
  z-index: 10000;
}

.modal-content.user-select-modal {
  background: white;
  border-radius: 8px;
  padding: 24px;
  width: 900px;
  max-width: 95%;
  height: 650px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.modal-title {
  margin: 0;
  font-size: 20px;
  font-weight: bold;
  color: #333;
}

.close-btn {
  background: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: #666;
}

/* 搜尋列 */
.search-bar {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
}

.search-input {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
  width: 150px;
}

.btn-search {
  background: #2f5a70;
  color: white;
  border: none;
  padding: 8px 24px;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
}

/* 左右佈局 */
.layout-body {
  display: flex;
  flex: 1;
  border-top: 1px solid #eee;
  overflow: hidden;
}

/* 左側邊欄 */
.sidebar {
  width: 220px;
  border-right: 1px solid #eee;
  padding: 16px 16px 16px 0;
  overflow-y: auto;
}

.sidebar-header {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  border-bottom: 1px solid #eee;
  font-weight: bold;
  font-size: 14px;
  margin-bottom: 12px;
}

.tree-menu,
.sub-menu {
  list-style: none;
  padding: 0;
  margin: 0;
  font-size: 13px;
  color: #333;
}

.sub-menu {
  padding-left: 16px;
}

.tree-menu li {
  padding: 6px 4px;
  cursor: pointer;
}

.tree-menu li:hover {
  background-color: #f5f5f5;
}

.arrow {
  color: #aaa;
  font-size: 10px;
  margin-right: 4px;
}

.tree-menu li.active {
  color: #2f5a70;
  font-weight: bold;
}

/* 右側表格區塊 */
.main-content {
  flex: 1;
  padding-left: 20px;
  display: flex;
  flex-direction: column;
}

.table-container {
  flex: 1;
  overflow-y: auto;
}

.user-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 8px;
  text-align: center;
  font-size: 13px;
}

.user-table th {
  color: #666;
  font-weight: normal;
  border-bottom: 1px solid #eee;
  padding-bottom: 12px;
}

.user-table td {
  padding: 12px 8px;
  background-color: #f9f9f9;
  color: #333;
  vertical-align: middle;
}

.user-table td:first-child {
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
}

.user-table td:last-child {
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
}

/* 實際照片頭像樣式 */
.user-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  object-fit: cover;
  display: block;
  margin: 0 auto;
}

.avatar-placeholder {
  width: 32px;
  height: 32px;
  background: #e2e8f0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  color: #94a3b8;
}

.btn-text-select {
  background: transparent;
  border: none;
  color: #0ea5e9;
  cursor: pointer;
  font-size: 13px;
  white-space: nowrap;
}

.btn-text-select:hover {
  text-decoration: underline;
}

/* 分頁 */
.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  padding-top: 16px;
  border-top: 1px solid #eee;
  font-size: 13px;
  color: #666;
  margin-top: auto;
}

.page-size {
  padding: 4px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.page-controls {
  display: flex;
  gap: 4px;
}

.page-btn {
  background: #f0f0f0;
  border: none;
  width: 28px;
  height: 28px;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.page-btn.active {
  background: #bce3f9;
  color: #0ea5e9;
  font-weight: bold;
}

.goto-input {
  width: 30px;
  padding: 4px;
  text-align: center;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin: 0 4px;
}
</style>
