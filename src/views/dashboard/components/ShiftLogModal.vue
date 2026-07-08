<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content shift-modal">
      <div class="modal-header">
        <h2 class="modal-title">交班日誌</h2>
        <button class="close-btn" @click="closeModal">✕</button>
      </div>

      <div class="sub-header">
        <div class="info-item">
          <span class="icon">📅</span> 2026/06/29
        </div>
        <div class="info-item">
          <span class="icon text-yellow">☀️</span> 白班 ( 07：00-15：00 )
        </div>
        <div class="divider-vertical"></div>
        <div class="info-item">
          <span class="icon">👤</span> 記錄人：王小美 護理師/照服員
        </div>
      </div>

      <div class="section-title">住民事件記錄</div>
      <div class="table-wrapper">
        <table class="shift-table">
          <thead>
            <tr>
              <th>時間</th>
              <th>重要度</th>
              <th>類別</th>
              <th>床號/姓名</th>
              <th>記錄人</th>
              <th>內容</th>
              <th>狀態追蹤</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="text-center">2026/06/29<br>10：00</td>
              <td class="text-center"><span class="tag tag-high">高</span></td>
              <td class="text-center">生理</td>
              <td class="text-center">102-B<br>張XX</td>
              <td class="text-center">王小美</td>
              <td>早上10：00體溫38.2度，已給予退燒藥，<br>14：26測量為37.4度。<br>下一班請每2小時量一次體溫。</td>
              <td>
                <select class="status-select outline-none">
                  <option>需持續追蹤</option>
                  <option>已處理/待觀察</option>
                </select>
              </td>
            </tr>
            <tr>
              <td class="text-center">2026/06/29<br>13：00</td>
              <td class="text-center"><span class="tag tag-med">中</span></td>
              <td class="text-center">事件</td>
              <td class="text-center">501-6<br>張XX</td>
              <td class="text-center">王小美</td>
              <td>下午13：10在床邊企圖自行下床時滑坐在地上，<br>無明顯外傷，意識清楚。<br>下一班請多注意下床意願。</td>
              <td>
                <select class="status-select outline-none">
                  <option>已處理/待觀察</option>
                  <option>需持續追蹤</option>
                </select>
              </td>
            </tr>
            <tr>
              <td class="text-center">2026/06/28<br>08：01</td>
              <td class="text-center"><span class="tag tag-low">低</span></td>
              <td class="text-center">事務</td>
              <td class="text-center">202-3<br>黃XX</td>
              <td class="text-center">李小明</td>
              <td>上午08：01入住X樓202室3床<br>下一班請多注意適應情況。</td>
              <td>
                <select class="status-select outline-none">
                  <option>已處理/待觀察</option>
                  <option>需持續追蹤</option>
                </select>
              </td>
            </tr>
            <tr>
              <td class="text-center">2026/06/26<br>23：16</td>
              <td class="text-center"><span class="tag tag-med">中</span></td>
              <td class="text-center">事件</td>
              <td class="text-center">402-3<br>康XX</td>
              <td class="text-center">張大中</td>
              <td>晚上23：16離開房間，已確認住民回到房間床<br>上入睡。</td>
              <td>
                <select class="status-select outline-none">
                  <option>已處理/待觀察</option>
                  <option>需持續追蹤</option>
                </select>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="bottom-grid">

        <div class="summary-section">
          <div class="section-title">今日已處置完成摘要</div>

          <div class="summary-card border-red">
            <div class="card-title text-red">SOS警報</div>
            <div class="card-desc">[2026/06/29 13：00] 501-6床 張XX 觸發跌倒警報<br><span
                class="remark">備註：在床邊試圖自行下床時滑坐在地上</span></div>
          </div>

          <div class="summary-card border-blue">
            <div class="card-title text-blue">低電量警報</div>
            <div class="card-desc">[2026/06/29 14：34] 408-3床 高XX 觸發低電量警報</div>
          </div>

          <div class="summary-card border-purple">
            <div class="card-title text-purple">夜間移動警報</div>
            <div class="card-desc">[2026/06/26 23：16] 402-3床 康XX 觸發夜間移動警報<br><span class="remark">備註：離開房間走動</span></div>
          </div>
        </div>

        <div class="form-section">
          <div class="form-box">
            <div class="section-title mt-0">新增住民事件記錄</div>
            <div class="form-row">
              <div class="input-group">
                <label>時間</label>
                <input type="text" value="2026/06/29" class="input-date" />
              </div>
              <div class="input-group">
                <label>重要度</label>
                <select class="input-select">
                  <option>🔴 高</option>
                </select>
              </div>
              <div class="input-group">
                <label>類別</label>
                <select class="input-select">
                  <option>類別</option>
                </select>
              </div>
              <div class="input-group flex-1">
                <label>床號/姓名</label>
                <div class="search-input-wrapper">
                  <input type="text" placeholder="輸入床號或姓名查詢" class="input-search" />
                  <span class="search-icon">🔍</span>
                </div>
              </div>
              <div class="input-group">
                <label>記錄人</label>
                <input type="text" value="王小美" class="input-recorder" readonly />
              </div>
            </div>

            <div class="input-group full-width mt-12">
              <label>內容</label>
              <textarea class="content-textarea"></textarea>
            </div>

            <div class="submit-row">
              <button class="btn-submit">新增</button>
            </div>
          </div>
        </div>

      </div>

    </div>
  </div>
</template>

<script setup>
defineProps({ isOpen: Boolean })
const emit = defineEmits(['close'])
const closeModal = () => emit('close')
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

.modal-content.shift-modal {
  background: white;
  border-radius: 8px;
  padding: 24px 32px;
  width: 1100px;
  max-width: 95%;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
}

/* 標題與關閉按鈕 */
.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
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

/* 次標題區塊 */
.sub-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-top: 12px;
  padding-bottom: 16px;
  border-bottom: 1px solid #eaeaea;
  font-size: 15px;
  color: #333;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 6px;
}

.text-yellow {
  color: #fadb14;
}

.divider-vertical {
  width: 1px;
  height: 16px;
  background-color: #ccc;
}

/* 區塊標題 */
.section-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin: 20px 0 12px 0;
}

.mt-0 {
  margin-top: 0;
}

/* 住民事件記錄表格 */
.table-wrapper {
  border: 1px solid #e5e7eb;
  border-radius: 6px;
  overflow: hidden;
}

.shift-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}

.shift-table th {
  background-color: #f9fafb;
  color: #4b5563;
  font-weight: normal;
  padding: 12px;
  border-bottom: 1px solid #e5e7eb;
  text-align: left;
}

.shift-table th.text-center,
.shift-table td.text-center {
  text-align: center;
}

.shift-table td {
  padding: 12px;
  border-bottom: 1px solid #e5e7eb;
  color: #1f2937;
  vertical-align: top;
  line-height: 1.5;
}

.shift-table tr:last-child td {
  border-bottom: none;
}

.status-select {
  border: none;
  background: transparent;
  color: #333;
  cursor: pointer;
}

.outline-none:focus {
  outline: none;
}

/* 重要度標籤 */
.tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  font-size: 14px;
  font-weight: bold;
}

.tag-high {
  background-color: #fee2e2;
  color: #ef4444;
}

.tag-med {
  background-color: #fef3c7;
  color: #f59e0b;
}

.tag-low {
  background-color: #e0f2fe;
  color: #3b82f6;
}

/* 底部左右區塊 */
.bottom-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
  margin-top: 8px;
}

/* 左側：摘要卡片 */
.summary-card {
  border: 1px solid #e5e7eb;
  border-radius: 6px;
  padding: 12px 16px;
  margin-bottom: 12px;
  background-color: white;
}

.summary-card.border-red {
  border-left: 4px solid #ef4444;
}

.summary-card.border-blue {
  border-left: 4px solid #3b82f6;
}

.summary-card.border-purple {
  border-left: 4px solid #8b5cf6;
}

.card-title {
  font-weight: bold;
  font-size: 15px;
  margin-bottom: 4px;
}

.text-red {
  color: #ef4444;
}

.text-blue {
  color: #3b82f6;
}

.text-purple {
  color: #8b5cf6;
}

.card-desc {
  font-size: 13px;
  color: #333;
  line-height: 1.5;
}

.remark {
  color: #6b7280;
  font-size: 12px;
}

/* 右側：新增表單 */
.form-box {
  background-color: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 6px;
  padding: 16px;
  margin-top: 20px;
}

.form-row {
  display: flex;
  gap: 12px;
  align-items: flex-end;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.input-group label {
  font-size: 14px;
  color: #333;
}

.input-group.flex-1 {
  flex: 1;
}

.input-group.full-width {
  width: 100%;
}

.mt-12 {
  margin-top: 12px;
}

.input-date,
.input-select,
.input-recorder,
.input-search {
  padding: 8px 10px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

.input-date {
  width: 110px;
}

.input-select {
  width: 80px;
}

.input-recorder {
  width: 80px;
  background-color: #f3f4f6;
  color: #6b7280;
}

.search-input-wrapper {
  position: relative;
  width: 100%;
}

.search-input-wrapper input {
  width: 100%;
  padding-right: 30px;
}

.search-icon {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  color: #9ca3af;
  font-size: 14px;
}

.content-textarea {
  width: 100%;
  height: 80px;
  padding: 10px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  resize: none;
  outline: none;
}

.submit-row {
  display: flex;
  justify-content: flex-end;
  margin-top: 12px;
}

.btn-submit {
  background-color: #2f5a70;
  color: white;
  border: none;
  padding: 8px 24px;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
}

.btn-submit:hover {
  background-color: #1e3a48;
}
</style>
