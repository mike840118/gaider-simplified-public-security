<template>
    <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
        <div class="modal-content file-modal">
            <button class="close-btn" @click="closeModal">✕</button>
            <h2 class="modal-title">修改檔案</h2>

            <div class="user-select-bar">
                <span>使用者姓名</span>
                <input type="text" placeholder="請選擇用戶" />
                <button class="btn-select">選擇</button>
            </div>

            <div class="table-container">
                <table class="medical-table">
                    <tbody>
                        <tr>
                            <td class="header-cell">姓名</td>
                            <td><input type="text" class="w-full" /></td>
                            <td class="header-cell">性別</td>
                            <td>
                                <label><input type="radio" name="fgender" /> 男</label>
                                <label class="ml-2"><input type="radio" name="fgender" /> 女</label>
                            </td>
                            <td class="header-cell">出生日期</td>
                            <td colspan="3"><input type="date" class="w-full" /></td>
                        </tr>
                        <tr>
                            <td class="header-cell">身份證號</td>
                            <td><input type="text" class="w-full" /></td>
                            <td class="header-cell">住址</td>
                            <td colspan="5"><input type="text" class="w-full" /></td>
                        </tr>
                        <tr>
                            <td class="header-cell">家庭電話</td>
                            <td><input type="text" class="w-full" /></td>
                            <td class="header-cell">緊急聯絡人姓名</td>
                            <td><input type="text" class="w-full" /></td>
                            <td class="header-cell">緊急聯絡人電話</td>
                            <td colspan="3"><input type="text" class="w-full" /></td>
                        </tr>
                        <tr>
                            <td class="header-cell">藥物過敏史</td>
                            <td colspan="7">
                                <label><input type="radio" name="allergy" /> 無</label>
                                <label class="ml-4"><input type="radio" name="allergy" /> 有：</label>
                                <label class="ml-2 text-gray"><input type="checkbox" disabled /> 青黴素</label>
                                <label class="ml-2 text-gray"><input type="checkbox" disabled /> 磺胺</label>
                                <label class="ml-2 text-gray"><input type="checkbox" disabled /> 鏈黴素</label>
                                <label class="ml-2 text-gray"><input type="checkbox" disabled /> 其它</label>
                            </td>
                        </tr>
                        <tr>
                            <td class="header-cell">暴露史</td>
                            <td colspan="7">
                                <label><input type="radio" name="exposure" /> 無</label>
                                <label class="ml-4"><input type="radio" name="exposure" /> 有：</label>
                                <label class="ml-2 text-gray"><input type="checkbox" disabled /> 化學品</label>
                                <label class="ml-2 text-gray"><input type="checkbox" disabled /> 毒物</label>
                                <label class="ml-2 text-gray"><input type="checkbox" disabled /> 射線</label>
                            </td>
                        </tr>

                        <tr>
                            <td class="header-cell" rowspan="3">一般狀況</td>
                            <td class="sub-header">體溫</td>
                            <td>
                                <div class="flex-center"><input type="text" class="w-half" /> °C</div>
                            </td>
                            <td class="sub-header">脈搏</td>
                            <td colspan="4">
                                <div class="flex-end"><input type="text" class="w-half" /> 次 / 分</div>
                            </td>
                        </tr>
                        <tr>
                            <td class="sub-header">空腹血糖</td>
                            <td>
                                <div class="flex-center"><input type="text" class="w-half" /> mg/dL</div>
                            </td>
                            <td class="sub-header">慣用手・血壓</td>
                            <td colspan="4">
                                <label><input type="radio" name="hand" /> 左側</label>
                                <label class="ml-2"><input type="radio" name="hand" /> 右側</label>
                                <span class="ml-4">舒張壓 / 收縮壓 <input type="text" class="w-20 ml-2" /> mmHg</span>
                            </td>
                        </tr>
                        <tr>
                            <td class="sub-header">身高</td>
                            <td>
                                <div class="flex-center"><input type="text" class="w-half" /> CM</div>
                            </td>
                            <td class="sub-header">體重</td>
                            <td colspan="4">
                                <div class="flex-end"><input type="text" class="w-half" /> KG</div>
                            </td>
                        </tr>

                        <tr>
                            <td class="header-cell" rowspan="4">既往史</td>
                            <td class="sub-header" rowspan="1">疾病</td>
                            <td colspan="6">
                                <div class="checkbox-grid mb-2">
                                    <label><input type="checkbox" /> 高壓力</label>
                                    <label><input type="checkbox" /> 糖尿病</label>
                                    <label><input type="checkbox" /> 冠心病</label>
                                    <label><input type="checkbox" /> 惡性腫瘤</label>
                                    <label><input type="checkbox" /> 腦出血、腦梗塞</label>
                                    <label><input type="checkbox" /> 精神分裂症</label>
                                    <label><input type="checkbox" /> 肝炎</label>
                                    <label><input type="checkbox" /> 頸椎病、腰椎病</label>
                                    <label><input type="checkbox" /> 骨性關節炎</label>
                                    <label><input type="checkbox" /> 腦梗塞</label>
                                    <label><input type="checkbox" /> 其他</label>
                                </div>
                                <div class="date-grid text-gray">
                                    <span><input type="checkbox" disabled /> 確診時間 ____ 年 __ 月</span>
                                    <span><input type="checkbox" disabled /> 確診時間 ____ 年 __ 月</span>
                                    <span><input type="checkbox" disabled /> 確診時間 ____ 年 __ 月</span>
                                    <span><input type="checkbox" disabled /> 確診時間 ____ 年 __ 月</span>
                                    <span><input type="checkbox" disabled /> 確診時間 ____ 年 __ 月</span>
                                    <span><input type="checkbox" disabled /> 確診時間 ____ 年 __ 月</span>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <td class="sub-header">手術</td>
                            <td colspan="6" class="history-rows">
                                <div class="flex-start"><label><input type="radio" name="surgery" /> 無</label></div>
                                <div class="flex-start"><label><input type="radio" name="surgery" /> 有：</label>
                                    <div class="ml-2 flex-col">
                                        <span>1、名稱 <input type="text" class="border-b" /> 時間 <input type="text"
                                                class="w-10 border-b" /> 年 <input type="text" class="w-10 border-b" />
                                            月</span>
                                        <span>2、名稱 <input type="text" class="border-b" /> 時間 <input type="text"
                                                class="w-10 border-b" /> 年 <input type="text" class="w-10 border-b" />
                                            月</span>
                                        <span>3、名稱 <input type="text" class="border-b" /> 時間 <input type="text"
                                                class="w-10 border-b" /> 年 <input type="text" class="w-10 border-b" />
                                            月</span>
                                    </div>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <td class="sub-header">重大外傷</td>
                            <td colspan="6" class="history-rows">
                                <div class="flex-start"><label><input type="radio" name="trauma" /> 無</label></div>
                                <div class="flex-start"><label><input type="radio" name="trauma" /> 有：</label>
                                    <div class="ml-2 flex-col">
                                        <span>1、名稱 <input type="text" class="border-b" /> 時間 <input type="text"
                                                class="w-10 border-b" /> 年 <input type="text" class="w-10 border-b" />
                                            月</span>
                                        <span>2、名稱 <input type="text" class="border-b" /> 時間 <input type="text"
                                                class="w-10 border-b" /> 年 <input type="text" class="w-10 border-b" />
                                            月</span>
                                    </div>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <td class="sub-header">輸血</td>
                            <td colspan="6" class="history-rows">
                                <div class="flex-start"><label><input type="radio" name="blood" /> 無</label></div>
                                <div class="flex-start"><label><input type="radio" name="blood" /> 有：</label>
                                    <div class="ml-2 flex-col">
                                        <span>1、名稱 <input type="text" class="border-b" /> 時間 <input type="text"
                                                class="w-10 border-b" /> 年 <input type="text" class="w-10 border-b" />
                                            月</span>
                                        <span>2、名稱 <input type="text" class="border-b" /> 時間 <input type="text"
                                                class="w-10 border-b" /> 年 <input type="text" class="w-10 border-b" />
                                            月</span>
                                    </div>
                                </div>
                            </td>
                        </tr>

                        <tr>
                            <td class="header-cell" rowspan="3">家族史</td>
                            <td class="sub-header">父親</td>
                            <td colspan="2"><input type="text" class="w-full" /></td>
                            <td class="sub-header">母親</td>
                            <td colspan="3"><input type="text" class="w-full" /></td>
                        </tr>
                        <tr>
                            <td class="sub-header">兄弟姐妹</td>
                            <td colspan="2"><input type="text" class="w-full" /></td>
                            <td class="sub-header">子女</td>
                            <td colspan="3"><input type="text" class="w-full" /></td>
                        </tr>
                        <tr>
                            <td colspan="7">
                                <div class="checkbox-grid">
                                    <label><input type="checkbox" /> 高壓力</label>
                                    <label><input type="checkbox" /> 糖尿病</label>
                                    <label><input type="checkbox" /> 冠心病</label>
                                    <label><input type="checkbox" /> 惡性腫瘤</label>
                                    <label><input type="checkbox" /> 過敏症</label>
                                    <label><input type="checkbox" /> 精神分裂症</label>
                                    <label><input type="checkbox" /> 結核症</label>
                                    <label><input type="checkbox" /> 肝炎</label>
                                    <label><input type="checkbox" /> 腦卒中 (腦出血、腦梗塞)</label>
                                    <label><input type="checkbox" /> 先天畸形</label>
                                    <label><input type="checkbox" /> 其他</label>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <td class="header-cell">遺傳病史</td>
                            <td colspan="7">
                                <label><input type="radio" name="hered" /> 無</label>
                                <label class="ml-4"><input type="radio" name="hered" /> 有</label>
                            </td>
                        </tr>
                        <tr>
                            <td class="header-cell">身心障礙</td>
                            <td colspan="7">
                                <div class="mb-1">
                                    <label><input type="checkbox" /> 無</label>
                                    <label class="ml-4"><input type="checkbox" /> 聽力障礙</label>
                                    <label class="ml-4"><input type="checkbox" /> 語音障礙</label>
                                    <label class="ml-4"><input type="checkbox" /> 肢體障礙</label>
                                    <label class="ml-4"><input type="checkbox" /> 視力障礙</label>
                                    <label class="ml-4"><input type="checkbox" /> 精神障礙</label>
                                    <label class="ml-4"><input type="checkbox" /> 智力障礙</label>
                                </div>
                                <div>障礙類別編碼：<input type="text" class="border-b" /></div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div class="modal-footer">
                <button class="btn-primary" @click="closeModal">儲存</button>
                <button class="btn-default" @click="closeModal">取消</button>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({ isOpen: Boolean })
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

.modal-content.file-modal {
    background: white;
    border-radius: 12px;
    padding: 24px 32px;
    position: relative;
    max-width: 1100px;
    width: 95%;
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
    margin-bottom: 20px;
    font-size: 20px;
}

.user-select-bar {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
    gap: 12px;
}

.user-select-bar input {
    padding: 6px 12px;
    border: 1px solid #ccc;
    border-radius: 4px;
    width: 200px;
}

.btn-select {
    background: #2f5a70;
    color: white;
    border: none;
    padding: 6px 20px;
    border-radius: 16px;
    cursor: pointer;
}

.table-container {
    border: 1px solid #ccc;
    overflow-x: auto;
}

.medical-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
}

.medical-table td {
    border: 1px solid #ddd;
    padding: 10px;
    vertical-align: middle;
}

.header-cell,
.sub-header {
    background-color: #f7f9fa;
    text-align: center;
    font-weight: bold;
    width: 100px;
    color: #333;
}

.sub-header {
    font-weight: normal;
}

.w-full {
    width: 100%;
    border: none;
    outline: none;
    background: transparent;
}

.w-half {
    width: 60px;
    border: none;
    border-bottom: 1px solid #ccc;
    text-align: center;
    outline: none;
}

.w-10 {
    width: 40px;
    text-align: center;
}

.w-20 {
    width: 80px;
    text-align: center;
}

.border-b {
    border: none;
    border-bottom: 1px solid #ccc;
    outline: none;
}

.ml-2 {
    margin-left: 8px;
}

.ml-4 {
    margin-left: 16px;
}

.mb-1 {
    margin-bottom: 4px;
}

.mb-2 {
    margin-bottom: 8px;
}

.text-gray {
    color: #aaa;
}

.flex-center {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 4px;
}

.flex-end {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: 4px;
}

.flex-start {
    display: flex;
    align-items: flex-start;
    margin-bottom: 8px;
}

.flex-col {
    display: flex;
    flex-direction: column;
    gap: 4px;
}

.checkbox-grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 8px;
}

.date-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
    margin-top: 8px;
}

.modal-footer {
    margin-top: 24px;
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