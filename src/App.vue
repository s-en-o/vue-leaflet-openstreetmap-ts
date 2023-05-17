<script setup lang="ts">
import 'leaflet/dist/leaflet.css';
import L from 'leaflet';
import type { LatLngExpression, Map } from 'leaflet';
import { ref, onMounted } from 'vue';
import homeIcon from '../src/assets/home.png';

const mapRef = ref<HTMLElement | null>(null);
const latLang: LatLngExpression = [-6.233624, 106.840086];

const createMap = (): void => {
    // Do nothing if the element is not created yet
    if (!mapRef.value) return;

    const map = L.map(mapRef.value, {
        center: latLang,
        zoom: 8,
    });

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution:
            '&copy; <a href="https://openstreetmap.org/copyright">OpenStreetMap contributors</a>',
    }).addTo(map);

    // Creating the marker
    createMarker(map, latLang);
};

const createMarker = (map: Map, location: LatLngExpression): void => {
    const marker = L.icon({
        iconUrl: homeIcon,
        iconSize: [40, 40],
        // iconAnchor: [22, 94],
        popupAnchor: [0, -20],
        // shadowUrl: homeIcon,
        // shadowSize: [68, 95],
        // shadowAnchor: [22, 94],
    });

    L.marker(location, { icon: marker })
        .addTo(map)
        .bindPopup('Jakarta')
        .openPopup();
};

onMounted(() => {
    createMap();
});
</script>

<template>
    <div class="map" ref="mapRef"></div>
</template>

<style scoped>
.map {
    height: 100vh;
    width: 100vw;
}
</style>
