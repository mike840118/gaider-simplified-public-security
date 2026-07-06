<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content admission-modal">
      <div class="modal-header">
        <h2 class="modal-title">{{ $t('admission.title') }}</h2>
        <button class="close-btn" @click="closeModal">✕</button>
      </div>

      <div class="modal-body">
        <div class="user-select-row">
          <input type="text" :value="selectedUserName" readonly :placeholder="$t('admission.select_user')" />
          <button class="btn-select" @click="isUserSelectOpen = true">{{ $t('common.select') }}</button>
        </div>

        <div class="form-grid">
          <div class="form-group">
            <label>{{ $t('admission.dept') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_dept') }}</option>
            </select>
          </div>
          <div class="form-group"></div>
          <div class="form-group">
            <label>{{ $t('admission.doctor') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_doctor') }}</option>
            </select>
          </div>
          <div class="form-group">
            <label>{{ $t('admission.nurse') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_nurse') }}</option>
            </select>
          </div>

          <div class="form-group">
            <label>{{ $t('admission.room') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_room') }}</option>
            </select>
          </div>
          <div class="form-group">
            <label>{{ $t('admission.bed') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_bed') }}</option>
            </select>
          </div>

          <div class="form-group">
            <label>{{ $t('admission.level') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_level') }}</option>
            </select>
          </div>
          <div class="form-group">
            <label>{{ $t('admission.admission_no') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_admission_no') }}</option>
            </select>
          </div>

          <div class="form-group">
            <label>{{ $t('admission.in_date') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_date') }}</option>
            </select>
          </div>
          <div class="form-group">
            <label>{{ $t('admission.out_date') }}</label>
            <select>
              <option>{{ $t('admission.placeholder_date') }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="modal-footer">
        <button class="btn-primary" @click="closeModal">{{ $t('common.confirm') }}</button>
        <button class="btn-default" @click="closeModal">{{ $t('common.cancel') }}</button>
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

const handleUserSelect = (user) => {
  selectedUserName.value = user.name
  isUserSelectOpen.value = false
}

const closeModal = () => {
  emit('close')
  setTimeout(() => {
    selectedUserName.value = ''
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

.modal-content.admission-modal {
  background: white;
  border-radius: 8px;
  padding: 24px 32px;
  width: 800px;
  max-width: 90%;
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

.user-select-row {
  display: flex;
  gap: 12px;
  margin-bottom: 24px;
  align-items: center;
}

.user-select-row input {
  width: 200px;
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

.btn-select {
  background: white;
  color: #2f5a70;
  border: 1px solid #2f5a70;
  padding: 8px 24px;
  border-radius: 4px;
  cursor: pointer;
}

.form-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  column-gap: 40px;
  row-gap: 20px;
}

.form-group {
  display: flex;
  align-items: center;
}

.form-group label {
  width: 80px;
  font-size: 14px;
  color: #333;
  flex-shrink: 0;
}

.form-group select {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  color: #666;
  outline: none;
}

.modal-footer {
  margin-top: 32px;
  display: flex;
  justify-content: center;
  gap: 16px;
}

.btn-primary {
  background: #2f5a70;
  color: white;
  border: none;
  padding: 10px 32px;
  border-radius: 20px;
  cursor: pointer;
}

.btn-default {
  background: white;
  color: #333;
  border: 1px solid #ccc;
  padding: 10px 32px;
  border-radius: 20px;
  cursor: pointer;
}
</style>
