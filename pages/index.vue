<template>
    <div>
      <LMap
        style="height:100vh; width:100vw"
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
    const crs = L.CRS.Simple;

    const bounds = computed(() =>
        [
            [0, 0],
            [height.value, width.value],
        ] as L.LatLngBoundsLiteral
    );
    const onMapReady = async () => {
        L.imageOverlay(imageOverlayUrl.value, bounds.value).setZIndex(-1).addTo(map.value.leafletObject)
        const base = await useLHeat({
            leafletObject: map.value?.leafletObject,
            heatPoints: sensors.value,
            // (optional) radius : default 50
            radius: 70
        });
        base.setOptions(
            {
                gradient: gradient, 
                blur: 56, 
                pane: "shadowPane",
                maxZoom: 3
            }
        )
    }

    const gradient = {
        0.1: '#011981', 
        0.2: '#0041ff', 
        0.3: '#0096ff',
        0.4: '#b9ebff',
        0.5: '#fffff0',
        0.6: '#fffff0',
        0.7: '#fbf505',
        0.8: '#ff9900',
        0.9: '#ff2a02',
        1: '#b41069',
    }

    // List of data provided by sensors, each object is a sensor.
    //lat = y(inverted), lng = x
    const sensors = ref([
        {
            lat: 71,
            lng: 3,
            intensity: .9
        }, {
            lat: 71,
            lng: 19,
            intensity: 0.7
        }, {
            lat: 71,
            lng: 35,
            intensity: 0.5
        }, {
            lat: 71,
            lng: 49.5,
            intensity: 0.3
        }, {
            lat: 71,
            lng: 63.5,
            intensity: 0.3
        }, {
            lat: 56,
            lng: 3,
            intensity: 0.7
        }, {
            lat: 56,
            lng: 19,
            intensity: 0.5
        }, {
            lat: 56,
            lng: 35,
            intensity: 0.3
        }, {
            lat: 56,
            lng: 49.5,
            intensity: 1
        }, {
            lat: 56,
            lng: 63,
            intensity: 0.8
        }, {
            lat: 41,
            lng: 3,
            intensity: 0.5
        }, {
            lat: 41,
            lng: 19,
            intensity: 0.5
        }, {
            lat: 41,
            lng: 35,
            intensity: 0.6
        }, {
            lat: 41,
            lng: 49.5,
            intensity: 0.4
        }, {
            lat: 41,
            lng: 63,
            intensity: 0.3
        }
    ]);
</script>