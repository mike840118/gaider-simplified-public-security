<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content worker-modal">
      <div class="modal-header">
        <h2 class="modal-title">{{ $t('worker_manage.title') }}</h2>
        <button class="close-btn" @click="closeModal">✕</button>
      </div>

      <div class="toolbar">
        <button class="btn-create">+ {{ $t('worker_manage.create_bed') }}</button>
        <select class="toolbar-select">
          <option>{{ $t('worker_manage.filter_floor') }}</option>
        </select>
        <select class="toolbar-select">
          <option>{{ $t('worker_manage.filter_room') }}</option>
        </select>
        <select class="toolbar-select">
          <option>{{ $t('worker_manage.filter_status') }}</option>
        </select>
        <input type="text" class="toolbar-search" :placeholder="$t('worker_manage.search_placeholder')" />
        <button class="btn-search-outline">{{ $t('common.search') }}</button>
      </div>

      <div class="cards-grid">
        <div class="worker-card" v-for="patient in mappedPatients" :key="patient.id">
          <div class="card-location">{{ patient.location }}</div>
          <img v-if="patient.avatar" :src="patient.avatar" class="avatar-img" alt="avatar" />
          <div v-else class="avatar-placeholder">👤</div>

          <div class="info-name">{{ patient.name }} &nbsp; <span class="age">{{ patient.age }}{{ $t('common.age')
          }}</span></div>
          <div class="info-row">{{ $t('worker_manage.care_level') }}：{{ patient.level }}</div>
          <div class="info-row">{{ $t('worker_manage.nurse') }}：{{ patient.nurse }}</div>

          <div class="card-actions">
            <select class="btn-action outline-none">
              <option value="" disabled selected>{{ $t('worker_manage.change_room') }}</option>
              <option v-for="room in roomOptions" :key="room" :value="room">{{ room }}</option>
            </select>
            <button class="btn-action" @click="showConfirmModal = true">{{ $t('worker_manage.checkout') }}</button>
          </div>
        </div>

        <div class="worker-card empty-card">
          <div class="card-location">XX樓-501室-2床</div>
          <div class="empty-actions">
            <button class="btn-action w-full mb-12">{{ $t('worker_manage.assign') }}</button>
            <button class="btn-action w-full">{{ $t('worker_manage.delete') }}</button>
          </div>
        </div>
      </div>

      <div v-if="showConfirmModal" class="confirm-overlay">
        <div class="confirm-modal">
          <h3>{{ $t('worker_manage.confirm_checkout') }}</h3>
          <p>{{ $t('worker_manage.cancel_checkout') }}</p>
          <div class="confirm-actions">
            <button class="btn-confirm-yes" @click="confirmCheckout">{{ $t('worker_manage.confirm_delete') }}</button>
            <button class="btn-confirm-no" @click="showConfirmModal = false">{{ $t('worker_manage.cancel_delete')
            }}</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import rawPatientData from '@/mock/patients.json'

defineProps({ isOpen: Boolean })
const emit = defineEmits(['close'])
const closeModal = () => emit('close')

const showConfirmModal = ref(false)
const roomOptions = ['X樓-202-3', 'X樓-104-5', 'X樓-304-1', 'X樓-304-2', 'X樓-304-3', 'X樓-304-4']

const confirmCheckout = () => {
  showConfirmModal.value = false
  alert('已退住')
}

const mappedPatients = computed(() => {
  const data = rawPatientData.data?.data || []
  return data.slice(0, 9).map(p => ({
    id: p.accountId,
    location: `${p.floorName || 'XX樓'}-${p.roomName || '201'}室-${p.bedName || '4'}床`,
    avatar: p.url || '',
    name: p.name || 'Williams',
    age: p.birthday ? (2026 - new Date(p.birthday).getFullYear()) : 40,
    level: 'II級',
    nurse: '李醫生'
  }))
})
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

.modal-content.worker-modal {
  background: white;
  border-radius: 8px;
  padding: 24px 32px;
  width: 1200px;
  max-width: 95%;
  height: 85vh;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
}

/* ================= 標題 ================= */
.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.modal-title {
  margin: 0;
  font-size: 24px;
  font-weight: bold;
  color: #333;
}

.close-btn {
  background: transparent;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #666;
}

/* ================= 工具列 ================= */
.toolbar {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 24px;
}

.btn-create {
  background-color: #2f5a70;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
}

.filter-icon {
  font-size: 18px;
  color: #999;
  margin: 0 4px;
}

.toolbar-select,
.toolbar-search {
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
  color: #555;
}

.toolbar-select {
  width: 120px;
}

.toolbar-search {
  width: 160px;
}

.btn-search-outline {
  background: transparent;
  color: #2f5a70;
  border: 1px solid #2f5a70;
  padding: 8px 24px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

/* ================= 卡片網格 (一排五張) ================= */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 16px;
  flex: 1;
  overflow-y: auto;
  padding-bottom: 16px;
}

.worker-card {
  border: 1px solid #94a3b8;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #fff;
}

.card-location {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-bottom: 16px;
  text-align: center;
}

/* 頭像 */
.avatar-img {
  width: 72px;
  height: 72px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 16px;
}

.avatar-placeholder {
  width: 72px;
  height: 72px;
  background-color: #e2e8f0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
  color: #94a3b8;
  margin-bottom: 16px;
}

/* 資訊文字 */
.info-name {
  font-size: 14px;
  color: #333;
  margin-bottom: 8px;
}

.age {
  font-size: 13px;
  color: #666;
}

.info-row {
  font-size: 13px;
  color: #333;
  margin-bottom: 4px;
}

/* 卡片按鈕 */
.card-actions {
  display: flex;
  justify-content: center;
  gap: 12px;
  width: 100%;
  margin-top: auto;
  padding-top: 16px;
}

.btn-action {
  background: transparent;
  border: 1px solid #666;
  color: #333;
  padding: 4px 16px;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
  text-align: center;
}

.outline-none {
  outline: none;
}

/* 空床位卡片特殊樣式 */
.empty-card {
  background-color: #f8fafc;
  border: 1px solid #cbd5e1;
}

.empty-actions {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex: 1;
  width: 60%;
}

.w-full {
  width: 100%;
}

.mb-12 {
  margin-bottom: 12px;
}

/* ================= 分頁器 ================= */
.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  padding-top: 20px;
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

.btn-action.outline-none {
  background: white;
  border: 1px solid #666;
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
}

.confirm-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
}

.confirm-modal {
  background: white;
  padding: 30px;
  border-radius: 8px;
  text-align: center;
  width: 300px;
}

.confirm-actions {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

.btn-confirm-yes {
  background: #2f5a70;
  color: white;
  border: none;
  padding: 8px 30px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-confirm-no {
  background: white;
  color: #666;
  border: 1px solid #ccc;
  padding: 8px 30px;
  border-radius: 4px;
  cursor: pointer;
}
</style>
