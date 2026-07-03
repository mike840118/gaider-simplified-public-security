<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content table-modal">
      <div class="modal-header">
        <h2 class="modal-title">{{ currentView === 'view' ? $t('dashboard.stats.low_battery') :
          $t('location.device_info') }}</h2>
        <button class="close-btn" @click="closeModal">✕</button>
      </div>

      <div v-if="currentView === 'view'" class="modal-body list-body">
        <div class="table-wrapper">
          <table class="battery-table">
            <thead>
              <tr>
                <th>{{ $t('data_manage.table.name') }}</th>
                <th>{{ $t('data_manage.table.location') }}</th>
                <th>{{ $t('data_manage.table.device_type') }}</th>
                <th>{{ $t('data_manage.table.battery_level') }}</th>
                <th>{{ $t('data_manage.table.time') }}</th>
                <th>{{ $t('data_manage.table.action') }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="device in deviceInfo.data" :key="device.accountId" class="table-row">
                <td>{{ formatUser(device) }}</td>
                <td>{{ formatLocation(device) }}</td>
                <td><strong>300B</strong>/{{ device.deviceCode }}</td>
                <td class="font-bold" :class="getBatteryColor(device.electricity)">
                  {{ device.electricity }}%
                </td>
                <td>{{ formatTime(device.electricity) }}</td>
                <td>
                  <button class="btn-text-blue" @click="goToReplace(device)">{{ $t('batteryDeviceModel.replace')
                  }}</button>
                </td>
              </tr>
              <tr v-if="!deviceInfo.data || deviceInfo.data.length === 0">
                <td colspan="6" class="text-center text-gray py-4">{{ $t('batteryDeviceModel.no_data') }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <div v-else class="modal-body replace-body">
        <div class="info-list" v-if="selectedDevice">
          <div class="info-row">
            <span class="label">{{ $t('batteryDeviceModel.device_code') }}</span>
            <span class="value">{{ selectedDevice.deviceCode }}</span>
          </div>
          <div class="info-row">
            <span class="label">{{ $t('batteryDeviceModel.bound_user') }}</span>
            <span class="value">{{ formatUser(selectedDevice) }}</span>
          </div>
        </div>

        <div class="warning-box">
          <span class="warning-icon">⚠️</span>
          {{ $t('batteryDeviceModel.warning') }}
        </div>

        <div class="form-group">
          <label>{{ $t('batteryDeviceModel.new_device_code') }}</label>
          <div class="input-wrapper">
            <select v-model="newDeviceCode" class="device-select">
              <option value="" disabled selected>{{ $t('batteryDeviceModel.new_device_placeholder') }}</option>
              <option v-for="code in availableDeviceCodes" :key="code" :value="code">
                {{ code }}
              </option>
            </select>
          </div>
        </div>

        <div class="modal-footer">
          <button class="btn-primary" @click="confirmReplace">{{ $t('batteryDeviceModel.replace_success') }}</button>
          <button class="btn-default" @click="currentView = 'view'">{{ $t('batteryDeviceModel.cancel') }}</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import { useI18n } from 'vue-i18n'
const { t } = useI18n()
const props = defineProps({
  isOpen: Boolean,
  deviceInfo: {
    type: Object,
    default: () => ({ data: [] })
  }
})

const emit = defineEmits(['close', 'replace'])

const currentView = ref('view')
const selectedDevice = ref(null)
const newDeviceCode = ref('')
const availableDeviceCodes = [
  '50:C0:F0:09:B1:2B',
  '50:C0:F0:FB:91:4E',
  '50:C0:F0:93:8D:E7',
  '50:V0:F0:33:9B:P8',
  '50:C0:G0:65:H5:1A'
]

// 格式化用戶名稱 (如: 108-C床 李XX)
const formatUser = (p) => {
  const roomStr = p.roomName ? `${p.roomName}-` : ''
  const bedStr = p.bedName ? `${p.bedName}床 ` : ''
  return `${roomStr}${bedStr}${p.name || '未知用戶'}`
}

// 格式化目前位置 (如: 一樓 108室)
const formatLocation = (p) => {
  if (!p.floorName && !p.roomName && !p.pointName) return '未知位置'
  const floor = p.floorName || ''
  let room = p.roomName ? `${p.roomName}室` : ''
  if (p.pointName) room = p.pointName // 若有特定地點名稱優先顯示
  return `${floor} ${room}`.trim()
}

// 根據電量決定顏色 class
const getBatteryColor = (level) => {
  if (level < 10) return 'text-red'
  if (level <= 20) return 'text-yellow'
  return 'text-green'
}

// 模擬圖片的時間顯示邏輯
const formatTime = (level) => {
  return level < 10 ? '10分鐘前' : '剛剛'
}

// 前往替換視圖
const goToReplace = (device) => {
  selectedDevice.value = device
  currentView.value = 'replace'
}

// 關閉 Modal 並重置狀態
const closeModal = () => {
  emit('close')
  setTimeout(() => {
    currentView.value = 'view'
    selectedDevice.value = null
    newDeviceCode.value = '' // 清空下拉選單的值
  }, 300)
}

const confirmReplace = () => {
  alert(t('batteryDeviceModel.replace_success_msg'))
  closeModal()
}

// 每次開啟時重置為列表畫面
watch(() => props.isOpen, (newVal) => {
  if (newVal) {
    currentView.value = 'view'
    selectedDevice.value = null
  }
})
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

/* 將 modal 加寬以容納表格 */
.modal-content.table-modal {
  background: white;
  border-radius: 12px;
  width: 780px;
  max-width: 95%;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  border-bottom: 1px solid #eee;
}

.modal-title {
  margin: 0;
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

.close-btn {
  background: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: #999;
}

.modal-body {
  padding: 24px;
}

.list-body {
  padding: 16px 24px 32px 24px;
}

.replace-body {
  width: 450px;
  margin: 0 auto;
}

/* 表格樣式 */
.table-wrapper {
  width: 100%;
  overflow-x: auto;
}

.battery-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 8px;
  /* 創造行與行之間的間距 */
  text-align: center;
  font-size: 14px;
}

.battery-table th {
  color: #666;
  font-weight: normal;
  border-bottom: 1px solid #eaeaea;
  padding-bottom: 12px;
}

.battery-table td {
  padding: 14px 10px;
  background-color: #f8f9fa;
  /* 淺灰色背景 */
  color: #333;
}

/* 讓表格列有圓角 */
.battery-table td:first-child {
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
}

.battery-table td:last-child {
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
}

/* 顏色輔助類別 */
.text-red {
  color: #ef4444;
}

.text-yellow {
  color: #f59e0b;
}

.text-green {
  color: #10b981;
}

.text-gray {
  color: #999;
}

.font-bold {
  font-weight: bold;
}

.btn-text-blue {
  background: transparent;
  border: none;
  color: #0ea5e9;
  cursor: pointer;
  font-size: 14px;
}

.btn-text-blue:hover {
  text-decoration: underline;
}

/* 替換畫面資訊列表 */
.info-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 24px;
}

.info-row {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  border-bottom: 1px dashed #eee;
  padding-bottom: 8px;
}

.info-row .label {
  color: #666;
}

.info-row .value {
  color: #333;
  font-weight: bold;
}

/* 警告與輸入框 */
.warning-box {
  background-color: #fffbeb;
  color: #d97706;
  padding: 12px;
  border-radius: 6px;
  font-size: 13px;
  line-height: 1.5;
  margin-bottom: 20px;
  display: flex;
  gap: 8px;
}

.form-group label {
  display: block;
  font-size: 14px;
  color: #333;
  margin-bottom: 8px;
  font-weight: bold;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.input-wrapper input {
  width: 100%;
  padding: 10px 36px 10px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
}

.icon-camera {
  position: absolute;
  right: 10px;
  color: #666;
  cursor: pointer;
  font-size: 16px;
}

/* 底部按鈕 */
.modal-footer {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-top: 24px;
}

.btn-primary {
  background: #0ea5e9;
  color: white;
  border: none;
  padding: 8px 32px;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
}

.btn-default {
  background: white;
  color: #666;
  border: 1px solid #ccc;
  padding: 8px 32px;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
}

/* 新增下拉式選單樣式 */
.device-select {
  width: 100%;
  padding: 10px 36px 10px 12px;
  /* 右側保留 padding 給預設箭頭或 icon */
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
  background-color: white;
  font-size: 14px;
  color: #333;
  cursor: pointer;
  appearance: none;
  /* 隱藏原生箭頭以利自訂，或可依需求移除此行 */
}

/* 若隱藏了原生箭頭，可以使用自訂的背景圖案，或是直接依賴瀏覽器預設樣式 */
.device-select:focus {
  border-color: #0ea5e9;
}
</style>
