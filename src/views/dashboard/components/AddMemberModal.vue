<template>
    <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
        <div class="modal-content add-member-modal">
            <button class="close-btn" @click="closeModal">✕</button>
            <h2 class="modal-title">新增會員資訊</h2>

            <div class="form-grid">
                <div class="form-section">
                    <h3>基本訊息</h3>
                    <div class="form-group avatar-group">
                        <label>大頭貼</label>
                        <div class="avatar-upload">+</div>
                    </div>
                    <div class="form-group">
                        <label>姓名</label>
                        <input type="text" placeholder="請輸入姓名" />
                    </div>
                    <div class="form-group inline-group">
                        <label><span class="text-red">*</span>性別</label>
                        <div class="radio-group">
                            <label><input type="radio" name="gender" value="male" checked /> 男</label>
                            <label><input type="radio" name="gender" value="female" /> 女</label>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>出生日期</label>
                        <input type="date" />
                    </div>
                    <div class="form-group">
                        <label>聯絡人</label>
                        <input type="text" placeholder="請輸入聯絡人" />
                    </div>
                    <div class="form-group">
                        <label><span class="text-red">*</span>電話區碼</label>
                        <select>
                            <option>請選擇電話區碼</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>聯絡電話</label>
                        <input type="text" placeholder="請輸入聯絡電話" />
                    </div>
                    <div class="form-group">
                        <label>聯絡地址</label>
                        <input type="text" placeholder="請輸入內容" />
                    </div>
                </div>

                <div class="form-section">
                    <h3>健康訊息</h3>
                    <div class="form-group with-unit">
                        <label>身高</label>
                        <div class="input-wrapper">
                            <input type="number" placeholder="請輸入身高" />
                            <span class="unit">cm</span>
                        </div>
                    </div>
                    <div class="form-group with-unit">
                        <label>體重</label>
                        <div class="input-wrapper">
                            <input type="number" placeholder="請輸入體重" />
                            <span class="unit">kg</span>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>收縮壓</label>
                        <input type="number" placeholder="請輸入收縮壓" />
                    </div>
                    <div class="form-group">
                        <label>舒張壓</label>
                        <input type="number" placeholder="請輸入舒張壓" />
                    </div>
                    <div class="form-group">
                        <label><span class="text-red">*</span>健康狀態</label>
                        <select>
                            <option>請選擇健康狀態</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>患者級別</label>
                        <select>
                            <option>請選擇患者級別</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label><span class="text-red">*</span>身障等級</label>
                        <select>
                            <option>請選擇身障等級</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label><span class="text-red">*</span>失能等級</label>
                        <select>
                            <option>請選擇失能等級</option>
                        </select>
                    </div>
                </div>

                <div class="form-section">
                    <h3>帳號訊息</h3>
                    <div class="form-group">
                        <label><span class="text-red">*</span>電子信箱</label>
                        <input type="email" placeholder="請輸入信箱" />
                    </div>
                    <div class="form-group">
                        <label><span class="text-red">*</span>密碼</label>
                        <input type="password" placeholder="請輸入密碼" />
                    </div>
                    <div class="form-group">
                        <label><span class="text-red">*</span>所屬機構</label>
                        <select>
                            <option>請選擇所屬機構</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>來源</label>
                        <input type="text" placeholder="請輸入設備MAC" />
                    </div>
                    <div class="form-group textarea-group">
                        <label>備註</label>
                        <textarea placeholder="請輸入內容"></textarea>
                    </div>
                </div>
            </div>

            <div class="modal-footer">
                <button class="btn-primary" @click="closeModal">確定</button>
                <button class="btn-default" @click="closeModal">取消</button>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    isOpen: Boolean
})
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
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
}

.modal-content {
    background: white;
    border-radius: 12px;
    padding: 24px 32px;
    position: relative;
    max-width: 1000px;
    width: 90%;
    max-height: 90vh;
    overflow-y: auto;
}

.close-btn {
    position: absolute;
    top: 20px;
    right: 20px;
    border: none;
    background: transparent;
    font-size: 20px;
    cursor: pointer;
}

.modal-title {
    margin-top: 0;
    margin-bottom: 24px;
    font-size: 20px;
}

.form-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 32px;
}

.form-section h3 {
    font-size: 16px;
    margin-bottom: 16px;
    text-align: center;
}

.form-group {
    display: flex;
    align-items: center;
    margin-bottom: 12px;
}

.form-group label {
    width: 80px;
    text-align: right;
    margin-right: 12px;
    font-size: 14px;
    color: #333;
    flex-shrink: 0;
}

.text-red {
    color: red;
    margin-right: 2px;
}

.form-group input:not([type="radio"]),
.form-group select,
.form-group textarea {
    flex: 1;
    padding: 6px 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
}

.avatar-group {
    align-items: flex-start;
}

.avatar-upload {
    width: 100px;
    height: 100px;
    border: 1px dashed #ccc;
    border-radius: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    color: #999;
    cursor: pointer;
}

.radio-group {
    display: flex;
    gap: 16px;
}

.with-unit .input-wrapper {
    display: flex;
    align-items: center;
    border: 1px solid #ccc;
    border-radius: 4px;
    flex: 1;
}

.with-unit input {
    border: none !important;
    flex: 1;
    outline: none;
}

.with-unit .unit {
    padding: 0 10px;
    background: #f5f5f5;
    border-left: 1px solid #ccc;
    font-size: 14px;
    color: #666;
    height: 100%;
    display: flex;
    align-items: center;
}

.textarea-group {
    align-items: flex-start;
}

.textarea-group textarea {
    height: 100px;
    resize: none;
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
    padding: 8px 32px;
    border-radius: 20px;
    cursor: pointer;
}

.btn-default {
    background: white;
    color: #333;
    border: 1px solid #ccc;
    padding: 8px 32px;
    border-radius: 20px;
    cursor: pointer;
}
</style>