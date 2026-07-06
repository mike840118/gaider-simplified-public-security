<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content bind-modal">
      <div class="modal-header">
        <h2 class="modal-title">{{ $t('bind_device.title') }}</h2>
        <button class="close-btn" @click="closeModal">✕</button>
      </div>

      <div class="modal-body">
        <div class="form-group">
          <label>{{ $t('bind_device.select_user') }}</label>
          <div class="input-with-btn">
            <input type="text" :value="selectedUserName" readonly :placeholder="$t('bind_device.placeholder_user')" />
            <button class="btn-select" @click="isUserSelectOpen = true">{{ $t('common.select') }}</button>
          </div>
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
          <button class="btn-confirm" @click="closeModal">{{ $t('bind_device.btn_confirm') }}</button>
        </div>
      </div>
    </div>

    <UserSelectModal :is-open="isUserSelectOpen" @close="isUserSelectOpen = false" @select="handleUserSelect" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import UserSelectModal from './UserSelectModal.vue' // 引入子彈窗

const props = defineProps({ isOpen: Boolean })
const emit = defineEmits(['close'])

const isUserSelectOpen = ref(false)
const selectedUserName = ref('')
const newDeviceCode = ref('')

const availableDeviceCodes = [
  '50:C0:F0:09:B1:2B',
  '50:C0:F0:FB:91:4E',
  '50:C0:F0:93:8D:E7',
  '50:V0:F0:33:9B:P8',
  '50:C0:G0:65:H5:1A'
]

const handleUserSelect = (user) => {
  selectedUserName.value = user.name
  isUserSelectOpen.value = false
}

const closeModal = () => {
  emit('close')
  // 延遲清空資料，避免關閉動畫時看到內容消失
  setTimeout(() => {
    selectedUserName.value = ''
    newDeviceCode.value = ''
  }, 300)
}
</script>
<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.modal-content.bind-modal {
  background: white;
  border-radius: 8px;
  padding: 24px 32px;
  width: 450px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
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

.form-group label {
  display: block;
  font-size: 14px;
  margin-bottom: 8px;
  color: #333;
}

.input-with-btn {
  display: flex;
  gap: 12px;
  align-items: center;
}

.input-with-btn input {
  flex: 1;
  padding: 10px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

.input-icon-wrapper {
  position: relative;
  flex: 1;
  display: flex;
  align-items: center;
}

.input-icon-wrapper input {
  width: 100%;
  padding-right: 36px;
}

.camera-icon {
  position: absolute;
  right: 10px;
  font-size: 18px;
  color: #999;
  cursor: pointer;
}

.btn-select {
  background: white;
  color: #2f5a70;
  border: 1px solid #2f5a70;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.mt-4 {
  margin-top: 20px;
}

.modal-footer {
  margin-top: 32px;
  display: flex;
  justify-content: center;
}

.btn-confirm {
  background: #2f5a70;
  color: white;
  border: none;
  padding: 10px 40px;
  border-radius: 20px;
  font-size: 15px;
  cursor: pointer;
  font-weight: bold;
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
