<template>
    <div class="outdoor-map-container">
        <div id="outdoor-map" class="map-element"></div>
    </div>
</template>

<script setup>
import { onMounted, onBeforeUnmount, watch } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

const props = defineProps({
    patients: { type: Array, required: true },
    selected: { type: Object, default: null }
})

let map = null
let markersGroup = null

// 自訂 Marker 樣式 (線上為綠色，離線為灰色，SOS為紅色閃爍)
const createCustomIcon = (patient) => {
    let color = patient.status === 'ONLINE' ? '#10b981' : '#9ca3af'
    let ringClass = ''

    if (patient.sos) {
        color = '#ef4444'
        ringClass = 'sos-ring' // SOS 閃爍特效
    }

    return L.divIcon({
        className: 'custom-leaflet-icon',
        html: `
            <div class="marker-pin" style="background-color: ${color};">
                <div class="${ringClass}"></div>
            </div>
            <div class="marker-name">${patient.name}</div>
        `,
        iconSize: [30, 42],
        iconAnchor: [15, 42]
    })
}

const renderMarkers = () => {
    if (!map) return
    markersGroup.clearLayers()

    props.patients.forEach(patient => {
        const marker = L.marker([patient.lat, patient.lng], {
            icon: createCustomIcon(patient)
        })
        marker.bindPopup(`<b>${patient.name}</b><br>狀態: ${patient.status === 'ONLINE' ? '在線' : '離線'}`)
        markersGroup.addLayer(marker)
    })
}

onMounted(() => {
    // 初始化地圖，中心點預設在台北市 101 附近
    map = L.map('outdoor-map').setView([25.033964, 121.564468], 13)

    // 使用完全免費的 OpenStreetMap 圖磚 (不需 API Key)
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors',
        maxZoom: 19,
    }).addTo(map)

    markersGroup = L.featureGroup().addTo(map)
    renderMarkers()
})

onBeforeUnmount(() => {
    if (map) map.remove()
})

// 監聽左側列表篩選，更新地圖上的標記
watch(() => props.patients, () => {
    renderMarkers()
}, { deep: true })

// 監聽左側點擊，將地圖中心平移至該使用者
watch(() => props.selected, (newVal) => {
    if (newVal && map) {
        map.flyTo([newVal.lat, newVal.lng], 16, { duration: 1 })
    }
})
</script>

<style>
.outdoor-map-container {
    width: 100%;
    height: 100%;
    position: relative;
}

.map-element {
    width: 100%;
    height: 100%;
    z-index: 1;
}

/* 自訂 Leaflet 標記的 CSS */
.custom-leaflet-icon {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    overflow: visible;
}

.marker-pin {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    border: 2px solid white;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.4);
    position: relative;
    z-index: 2;
}

.marker-name {
    background: rgba(255, 255, 255, 0.9);
    padding: 2px 6px;
    border-radius: 4px;
    font-size: 12px;
    font-weight: bold;
    color: #333;
    margin-top: 4px;
    white-space: nowrap;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
    z-index: 3;
}

/* SOS 閃爍動畫 */
.sos-ring {
    position: absolute;
    top: -6px;
    left: -6px;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    background-color: rgba(239, 68, 68, 0.5);
    animation: pulse 1s infinite ease-out;
    z-index: 1;
}

@keyframes pulse {
    0% {
        transform: scale(0.5);
        opacity: 1;
    }

    100% {
        transform: scale(2);
        opacity: 0;
    }
}
</style>