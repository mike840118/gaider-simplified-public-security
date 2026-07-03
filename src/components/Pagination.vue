<template>
  <div class="table-footer" v-if="total > 0">
    <span>{{ $t('pagination.total', { count: total }) }}</span>

    <select class="page-size" disabled>
      <option>{{ $t('pagination.page_size', { size: pageSize }) }}</option>
    </select>

    <div class="pagination-row">
      <button @click="changePage(current - 1)" :disabled="current === 1">&lt;</button>

      <template v-for="(item, index) in visiblePages" :key="item === '...' ? 'ellipsis-' + index : 'page-' + item">
        <span v-if="item === '...'" class="page-ellipsis">...</span>
        <button v-else :class="{ active: Number(current) === Number(item) }" @click="changePage(item)">
          {{ item }}
        </button>
      </template>

      <button @click="changePage(current + 1)" :disabled="current === totalPages">&gt;</button>
    </div>

    <span>{{ $t('pagination.goto') }}
      <input type="number" :value="current" @change="handleJump" class="goto-input" />
      {{ $t('pagination.page') }}
    </span>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  total: { type: Number, required: true },
  current: { type: Number, required: true },
  pageSize: { type: Number, default: 10 }
})

const emit = defineEmits(['change'])

const totalPages = computed(() => Math.ceil(props.total / props.pageSize) || 1)

// 計算應該顯示的頁碼陣列 (不變)
const visiblePages = computed(() => {
  const total = totalPages.value;
  const current = props.current;

  if (total <= 7) {
    return Array.from({ length: total }, (_, i) => i + 1);
  }

  if (current <= 4) {
    return [1, 2, 3, 4, 5, '...', total];
  }

  if (current >= total - 3) {
    return [1, '...', total - 4, total - 3, total - 2, total - 1, total];
  }

  return [1, '...', current - 1, current, current + 1, '...', total];
})

const changePage = (page) => {
  const pageNum = Number(page);
  if (pageNum >= 1 && pageNum <= totalPages.value) {
    emit('change', pageNum)
  }
}

const handleJump = (event) => {
  let val = parseInt(event.target.value)
  if (isNaN(val) || val < 1) val = 1
  if (val > totalPages.value) val = totalPages.value
  changePage(val)
}
</script>

<style scoped>
.table-footer {
  margin-top: auto;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 12px;
  font-size: 13px;
  color: #666;
  padding-top: 16px;
  border-top: 1px solid #e2e8f0;
  flex-shrink: 0;
}

.page-size {
  padding: 4px;
  border-radius: 4px;
  border: 1px solid #ccc;
  background: #f8fafc;
}

.pagination-row {
  display: flex;
  gap: 4px;
}

.pagination-row button {
  padding: 4px 10px;
  border: 1px solid #ccc;
  background: white;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.1s ease;
  /* 縮短過渡時間，讓點擊變色更即時 */
}

.pagination-row button:hover:not(:disabled) {
  background: #f1f5f9;
}

.pagination-row button:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

/* 確保當前作用中的頁碼能精準上色 */
.pagination-row button.active {
  background-color: #3b82f6 !important;
  color: white !important;
  border-color: #3b82f6 !important;
}

.page-ellipsis {
  padding: 4px 6px;
  color: #999;
  display: flex;
  align-items: flex-end;
  letter-spacing: 1px;
}

.goto-input {
  width: 40px;
  text-align: center;
  padding: 2px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
}
</style>
