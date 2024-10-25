<template>
    <div>
      <LMap
        style="height:100vh; 
        width:100vw"
        id="heatmap"
        ref="map"
        :zoom="zoom"
        :max-zoom="4"
        :crs="crs"
        :center="[50, 50]"
        :use-global-leaflet="true"
        @ready="onMapReady"
      >
      </LMap>
    </div>
</template>
  
<script lang="ts" setup>
    import { ref } from 'vue'
    import L from 'leaflet'
    import { LMap } from "@vue-leaflet/vue-leaflet";

    const map = ref(null) as any;
    const zoom = ref(3)
    const imageOverlayUrl = ref("/bm-canvas-image.png")
    const width = ref(100);
    const height = ref(100);
    const radius = ref(100)
    const intensity = ref(2000.0)
    const crs = L.CRS.Simple;

    const bounds = computed(() =>
        [
        [0, 0],
        [height.value, width.value],
        ] as L.LatLngBoundsLiteral
    );
    const onMapReady = async () => {
        const heat = await useLHeat({
            leafletObject: map.value?.leafletObject,
            heatPoints: heatPoints.value,
            // (optional) radius : default 50
        });
        L.imageOverlay(imageOverlayUrl.value, bounds.value).setZIndex(-1).addTo(map.value.leafletObject)
        L.rectangle(bounds.value,{stroke: false}).addTo(map.value.leafletObject)
    }

    // Create heat data
    const heatPoints = ref([{
        lat: 49,
        lng: 58,
        intensity: 2000
    }, {
        lat: 60.4,
        lng: 60.7,
        intensity: 1000
    }, {
        lat: 45.5,
        lng: 80.5,
        intensity: 2500
    }, {
        lat: 52.5,
        lng: 82.5,
        intensity: 1250
    }, {
        lat: 50,
        lng: 91,
        intensity: 2000
    }]);
</script>