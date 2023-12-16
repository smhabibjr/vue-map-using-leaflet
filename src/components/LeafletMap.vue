<template>
    <div>
        <h3>An interactive leaflet map</h3>
        <div id="map" style="height:90vh;"></div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import "leaflet/dist/leaflet.css"
import * as L from 'leaflet'
import 'leaflet.markercluster/dist/MarkerCluster.css';
import 'leaflet.markercluster/dist/MarkerCluster.Default.css';
import "leaflet.markercluster";
import { addressPoints } from './markerDemo'
import icon from './icon.png'
import markerShadow from './marker-shadow.png'

const initialMap = ref(null);

const myIcon = L.icon({
    iconUrl: icon,
    iconSize: [30, 30],
    iconAnchor: [22, 94],
    popupAnchor: [-3, -76],
    shadowUrl: markerShadow,
    shadowSize: [60, 30],
    shadowAnchor: [22, 94]
});

onMounted(()=> {
    initialMap.value = L.map('map', {zoomControl: true,zoom:1,zoomAnimation:false,fadeAnimation:true,markerZoomAnimation:true}).setView([23.8041, 90.4152], 6);
    L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19, 
        attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(initialMap.value);

    L.marker([24.3746, 88.6004], {icon: myIcon}).addTo(initialMap.value);
    L.marker([22.3752, 91.8349], {icon: myIcon}).addTo(initialMap.value);

    const markers = L.markerClusterGroup();

    addressPoints.forEach((element, index) => {
        markers
        .addLayer(L.marker([element.latitude, element.longitude], {icon: myIcon})
        .bindPopup(`<strong> Hello Bangladesh! </strong> <br> I am a popup.`));
    });

    initialMap.value.addLayer(markers);

});
</script>

<style scoped>

</style>