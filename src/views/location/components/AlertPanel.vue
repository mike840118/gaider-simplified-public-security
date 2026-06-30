<template>
    <div class="alert-panel-container">
        <div class="filter-bar">
            <input type="text" class="filter-input" placeholder="使用者姓名" />
            <input type="text" class="filter-input" placeholder="請輸入設備MAC" />
            <div class="date-input-wrapper">
                <span class="icon-calendar">📅</span>
                <input type="date" class="filter-date" value="2026-05-31" />
            </div>
            <div class="date-input-wrapper">
                <span class="icon-calendar">📅</span>
                <input type="date" class="filter-date" value="2026-06-30" />
            </div>
            <button class="btn btn-search">
                <span class="icon">🔍</span> 搜尋
            </button>
            <button class="btn btn-export">
                <span class="icon">📥</span> 導出
            </button>
        </div>

        <div class="table-container">
            <table class="alert-table">
                <thead>
                    <tr>
                        <th width="5%">序號</th>
                        <th width="15%">警報時間</th>
                        <th width="15%">姓名</th>
                        <th width="12%">聯絡電話</th>
                        <th width="18%">來源</th>
                        <th width="10%">設備型號</th>
                        <th width="10%">處理狀態</th>
                        <th width="10%">處理記錄</th>
                        <th width="5%">操作</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in mockAlerts" :key="item.id">
                        <td class="text-center">{{ item.id }}</td>
                        <td class="text-center">{{ item.time }}</td>
                        <td class="text-center">{{ item.name }}</td>
                        <td class="text-center">{{ item.phone }}</td>
                        <td class="text-center">{{ item.mac }}</td>
                        <td class="text-center">{{ item.model }}</td>
                        <td class="text-center text-red">{{ item.status }}</td>
                        <td class="text-center">{{ item.record }}</td>
                        <td class="text-center">
                            <a href="#" class="edit-link" @click.prevent="">✎ 編輯</a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <div class="pagination-bar">
            <span class="total-count">共 405 項</span>
            <select class="page-select">
                <option>10項/頁</option>
            </select>

            <div class="page-numbers">
                <button class="page-btn text-gray">&lt;</button>
                <button class="page-btn active">1</button>
                <button class="page-btn">2</button>
                <button class="page-btn">3</button>
                <button class="page-btn">4</button>
                <button class="page-btn">5</button>
                <button class="page-btn">6</button>
                <span class="page-dots">...</span>
                <button class="page-btn">41</button>
                <button class="page-btn">&gt;</button>
            </div>

            <div class="page-jump">
                前往 <input type="number" class="jump-input" value="1" /> 頁
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

// 依照截圖建立的假資料
const mockAlerts = ref([
    { id: 1, time: '2026-06-30 06:20:51', name: '蘇美英P1521586', phone: '919551832', mac: '863204051040958', model: '900P', status: '未處理', record: '' },
    { id: 2, time: '2026-06-28 14:15:42', name: '范小姐', phone: '935238223', mac: '863204051032013', model: '900P', status: '未處理', record: '' },
    { id: 3, time: '2026-06-28 12:49:26', name: '賴文來', phone: 'L1310289', mac: '863204051038341', model: '900P', status: '未處理', record: '' },
    { id: 4, time: '2026-06-28 12:42:56', name: '賴文來', phone: 'L1310289', mac: '863204051038341', model: '900P', status: '未處理', record: '' },
    { id: 5, time: '2026-06-28 09:47:03', name: '趙利民', phone: 'P1240687', mac: '863204051039786', model: '900P', status: '未處理', record: '' },
    { id: 6, time: '2026-06-27 16:20:40', name: '高清潭', phone: '933131880', mac: 'C8:5F:0B:35:58:...', model: '300B', status: '未處理', record: '' },
    { id: 7, time: '2026-06-27 10:39:26', name: '余豪俊', phone: '919426648', mac: '863204051034068', model: '900P', status: '未處理', record: '' }
])
</script>

<style scoped>
.alert-panel-container {
    background-color: #fff;
    border-radius: 8px;
    border: 1px solid #e2e8f0;
    padding: 20px;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
}

/* 頂部搜尋區 */
.filter-bar {
    display: flex;
    gap: 12px;
    margin-bottom: 24px;
    align-items: center;
}

.filter-input,
.filter-date {
    padding: 8px 12px;
    border: 1px solid #d9d9d9;
    border-radius: 4px;
    font-size: 14px;
    color: #333;
    outline: none;
}

.filter-input {
    width: 180px;
}

.filter-input::placeholder {
    color: #bfbfbf;
}

.date-input-wrapper {
    position: relative;
    display: flex;
    align-items: center;
}

.date-input-wrapper .icon-calendar {
    position: absolute;
    left: 10px;
    font-size: 14px;
    color: #bfbfbf;
    pointer-events: none;
}

.filter-date {
    padding-left: 32px;
    width: 130px;
    color: #555;
}

.btn {
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    color: white;
    font-size: 14px;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 6px;
    transition: opacity 0.2s;
}

.btn:hover {
    opacity: 0.8;
}

.btn-search,
.btn-export {
    background-color: #3bc8f6;
    /* 截圖中的亮藍色 */
}

/* 表格區 */
.table-container {
    flex: 1;
    overflow-y: auto;
}

.alert-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
    color: #333;
}

.alert-table th,
.alert-table td {
    padding: 16px 8px;
    border-bottom: 1px solid #f0f0f0;
}

.alert-table th {
    font-weight: bold;
    color: #262626;
}

.text-center {
    text-align: center;
}

.text-red {
    color: #ff4d4f;
}

.edit-link {
    color: #1890ff;
    text-decoration: none;
    font-size: 13px;
}

.edit-link:hover {
    text-decoration: underline;
}

/* 分頁區 */
.pagination-bar {
    display: flex;
    align-items: center;
    margin-top: 20px;
    font-size: 13px;
    color: #666;
}

.total-count {
    margin-right: 16px;
}

.page-select {
    padding: 4px 8px;
    border: 1px solid #d9d9d9;
    border-radius: 4px;
    margin-right: 24px;
    color: #666;
}

.page-numbers {
    display: flex;
    gap: 4px;
    margin-right: 24px;
}

.page-btn {
    min-width: 32px;
    height: 32px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid #d9d9d9;
    background: white;
    border-radius: 4px;
    cursor: pointer;
    color: #333;
}

.page-btn.active {
    background: #3bc8f6;
    color: white;
    border-color: #3bc8f6;
}

.page-btn.text-gray {
    color: #ccc;
    cursor: not-allowed;
}

.page-dots {
    display: flex;
    align-items: flex-end;
    padding: 0 4px;
}

.page-jump {
    display: flex;
    align-items: center;
    gap: 8px;
}

.jump-input {
    width: 40px;
    height: 32px;
    text-align: center;
    border: 1px solid #d9d9d9;
    border-radius: 4px;
}
</style>