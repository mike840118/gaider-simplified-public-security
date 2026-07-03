<template>
  <div class="analysis-tab-wrapper">
    <div class="filter-bar">
      <div class="date-input-wrapper">
        <span class="icon-calendar">📅</span>
        <input type="date" class="filter-date" value="2026-06-23" />
      </div>
      <span style="color: #666;">{{ $t('analysis_tab.to') }}</span>
      <div class="date-input-wrapper">
        <span class="icon-calendar">📅</span>
        <input type="date" class="filter-date" value="2026-06-30" />
      </div>
      <button class="btn btn-search"><span class="icon">🔍</span> {{ $t('common.search') }}</button>

      <div class="toggle-switch">
        <span class="active">{{ $t('analysis_tab.dynamic') }}</span>
        <label class="switch"><input type="checkbox" checked><span class="slider round"></span></label>
        <span>{{ $t('analysis_tab.static') }}</span>
      </div>
    </div>

    <div class="summary-cards">
      <div class="summary-card">
        <div class="card-label">{{ $t('analysis_tab.total_measurements') }}</div>
        <div class="card-value">{{ summaryStats.total }} <span class="unit">{{ $t('analysis_tab.unit_times') }}</span>
        </div>
      </div>
      <div class="summary-card">
        <div class="card-label text-red">{{ $t('analysis_tab.abnormal_count') }}</div>
        <div class="card-value text-red">{{ summaryStats.abnormal }} <span class="unit">{{ $t('analysis_tab.unit_times')
            }}</span></div>
      </div>
      <div class="summary-card">
        <div class="card-label text-orange">{{ $t('analysis_tab.abnormal_rate') }}</div>
        <div class="card-value text-orange">{{ summaryStats.rate }} <span class="unit">%</span></div>
      </div>
    </div>

    <div class="charts-container-layout">
      <div class="chart-box-left">
        <h4 class="chart-inner-title">{{ $t('analysis_tab.status_ratio') }}</h4>
        <div ref="statusPieChartRef" class="echart-element"></div>
      </div>
      <div class="chart-box-right">
        <h4 class="chart-inner-title">{{ $t('analysis_tab.trend_daily_avg') }}</h4>
        <div ref="trendLineChartRef" class="echart-element"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, nextTick, computed } from 'vue'
import { useI18n } from 'vue-i18n'
import * as echarts from 'echarts'

const props = defineProps({
  patients: Array,
  metric: String,
  selectedUserId: Number
})

const { t } = useI18n()
const statusPieChartRef = ref(null)
const trendLineChartRef = ref(null)

let statusPieChart = null
let trendLineChart = null

const metricName = computed(() => {
  const map = {
    'bp': t('data_manage.tabs.bp'),
    'spo2': t('data_manage.tabs.spo2'),
    'hr': t('data_manage.tabs.hr'),
    'temp': t('data_manage.tabs.temp'),
    'hrv': t('data_manage.tabs.hrv')
  }
  return map[props.metric] || ''
})

const computedAnalysis = computed(() => {
  let list = props.patients || []
  if (props.selectedUserId) list = list.filter(p => p.accountId === props.selectedUserId)

  let total = 0
  let abnormal = 0
  let normalCount = 0
  let warnCount = 0
  let urgentCount = 0

  const dateKeys = ['2026-06-23', '2026-06-24', '2026-06-25', '2026-06-26', '2026-06-27', '2026-06-28', '2026-06-29', '2026-06-30']
  let trendMap = {}
  dateKeys.forEach(d => { trendMap[d] = { sum: 0, count: 0 } })

  list.forEach(p => {
    const records = p.records ? (p.records[props.metric] || []) : []
    records.forEach(r => {
      total++
      if (r.level === 'URGENT') {
        abnormal++
        urgentCount++
      } else if (r.level === 'WARN') {
        abnormal++
        warnCount++
      } else {
        normalCount++
      }

      let numVal = 0
      if (props.metric === 'bp') numVal = r.sbp
      else if (props.metric === 'spo2') numVal = r.bo
      else if (props.metric === 'hr') numVal = r.hb
      else if (props.metric === 'temp') numVal = parseFloat(r.bodyTemp)
      else if (props.metric === 'hrv') numVal = r.hrv

      const dayStr = r.testTime.substring(0, 10)
      if (trendMap[dayStr]) {
        trendMap[dayStr].sum += numVal
        trendMap[dayStr].count++
      }
    })
  })

  let trendValues = dateKeys.map(d => {
    const item = trendMap[d]
    return item.count > 0 ? parseFloat((item.sum / item.count).toFixed(1)) : 0
  })

  const rate = total > 0 ? ((abnormal / total) * 100).toFixed(1) : '0.0'

  return {
    total, abnormal, rate,
    pieData: [
      { value: normalCount, name: t('status.normal') },
      { value: warnCount, name: t('status.warn') },
      { value: urgentCount, name: t('status.urgent') }
    ],
    trendX: dateKeys.map(d => d.substring(5)),
    trendY: trendValues
  }
})

const summaryStats = computed(() => {
  return {
    total: computedAnalysis.value.total,
    abnormal: computedAnalysis.value.abnormal,
    rate: computedAnalysis.value.rate
  }
})

const renderCharts = () => {
  if (!statusPieChartRef.value || !trendLineChartRef.value) return
  const data = computedAnalysis.value

  if (!statusPieChart) statusPieChart = echarts.init(statusPieChartRef.value)
  statusPieChart.setOption({
    tooltip: { trigger: 'item', formatter: '{b}: {c} ({d}%)' },
    legend: { bottom: '0', left: 'center', icon: 'circle' },
    color: ['#10b981', '#f59e0b', '#ef4444'],
    series: [{
      name: t('analysis_tab.status_ratio'),
      type: 'pie',
      radius: '50%',
      center: ['50%', '45%'],
      data: data.pieData,
      emphasis: {
        itemStyle: { shadowBlur: 10, shadowOffsetX: 0, shadowColor: 'rgba(0, 0, 0, 0.5)' }
      }
    }]
  }, true)

  if (!trendLineChart) trendLineChart = echarts.init(trendLineChartRef.value)
  trendLineChart.setOption({
    tooltip: { trigger: 'axis' },
    grid: { left: '4%', right: '4%', bottom: '10%', containLabel: true },
    xAxis: {
      type: 'category',
      boundaryGap: false,
      data: data.trendX,
      axisTick: { show: false }
    },
    yAxis: {
      type: 'value',
      scale: true,
      splitLine: { lineStyle: { color: '#f1f5f9' } }
    },
    series: [{
      name: metricName.value,
      type: 'line',
      data: data.trendY,
      smooth: true,
      symbol: 'circle',
      symbolSize: 8,
      itemStyle: { color: '#0ea5e9' },
      lineStyle: { width: 3 },
      areaStyle: {
        color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
          { offset: 0, color: 'rgba(14, 165, 233, 0.3)' },
          { offset: 1, color: 'rgba(14, 165, 233, 0.0)' }
        ])
      }
    }]
  }, true)
}

onMounted(() => nextTick(() => renderCharts()))
watch(() => computedAnalysis.value, () => renderCharts(), { deep: true })
</script>

<style scoped>
.analysis-tab-wrapper {
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow-y: auto;
  gap: 16px;
  padding-right: 4px;
}

/* 頂部工具列 */
.filter-bar {
  display: flex;
  gap: 12px;
  align-items: center;
  border-bottom: 1px solid #e2e8f0;
  padding-bottom: 16px;
  flex-shrink: 0;
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
}

.filter-date {
  padding: 8px 12px 8px 32px;
  border: 1px solid #d9d9d9;
  border-radius: 4px;
  font-size: 14px;
  color: #555;
  outline: none;
}

.btn-search {
  background-color: #3bc8f6;
  color: white;
  border: none;
  padding: 8px 24px;
  border-radius: 20px;
  cursor: pointer;
  font-weight: bold;
}

/* Switch Toggle */
.toggle-switch {
  margin-left: auto;
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
  color: #999;
}

.toggle-switch .active {
  color: #3bc8f6;
  font-weight: bold;
}

.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 20px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  transition: .4s;
  border-radius: 34px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 16px;
  width: 16px;
  left: 2px;
  bottom: 2px;
  background-color: white;
  transition: .4s;
  border-radius: 50%;
}

input:checked+.slider {
  background-color: #3bc8f6;
}

input:checked+.slider:before {
  transform: translateX(20px);
}

/* 數據卡片區 */
.summary-cards {
  display: flex;
  gap: 16px;
  flex-shrink: 0;
}

.summary-card {
  flex: 1;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 16px 20px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.card-label {
  font-size: 14px;
  color: #64748b;
  font-weight: bold;
}

.card-value {
  font-size: 26px;
  font-weight: bold;
  color: #1e293b;
}

.unit {
  font-size: 13px;
  font-weight: normal;
  margin-left: 2px;
  color: #64748b;
}

/* 顏色修飾 */
.text-red {
  color: #ef4444 !important;
}

.text-orange {
  color: #f59e0b !important;
}

/* 下方圖表配置佈局 */
.charts-container-layout {
  display: flex;
  gap: 20px;
  flex: 1;
  min-height: 340px;
}

.chart-box-left {
  flex: 1;
  background: white;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  flex-direction: column;
}

.chart-box-right {
  flex: 2;
  /* 折線圖寬度為圓餅圖的兩倍 */
  background: white;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  flex-direction: column;
}

.chart-inner-title {
  margin: 0 0 12px 0;
  font-size: 15px;
  font-weight: bold;
  color: #334155;
}

.echart-element {
  flex: 1;
  width: 100%;
  height: 100%;
}
</style>
