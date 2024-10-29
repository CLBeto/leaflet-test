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
    import 'simpleheat'

    const map = ref(null) as any;
    const zoom = ref(4)
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
            radius: 75
        });
        base.setOptions(
            {
                gradient: gradient, 
                blur: 55, 
                pane: "shadowPane",
                maxZoom: 3
            }
        )
    }

    const gradient = {
        // 0.1: '#011981', 
        // 0.2: '#0041ff', 
        0.1: '#0096ff',
        0.4: '#b9ebff',
        0.5: '#fffff0',
        0.65: '#fffff0',
        0.7: '#fbf505',
        0.8: '#ff9900',
        0.9: '#ff2a02',
        1: '#b41069',
    }

    // List of data provided by sensors, each object is a sensor.
    //lat = y(inverted), lng = x
    const sensors = ref([
         {
            lat: 49,
            lng: 58,
            intensity: 1
        }, {
            lat: 60,
            lng: 61,
            intensity: 0.8
        }, {
            lat: 45.5,
            lng: 81,
            intensity: 0.7
        }, {
            lat: 52.5,
            lng: 83,
            intensity: 0.7
        },{
            lat: 50,
            lng: 90.5,
            intensity: 0.7
        }, {
            lat: 56,
            lng: 92.5,
            intensity: 0.5
        },{
            lat: 71,
            lng: 95.5,
            intensity: 0.5
        }, {
            lat: 34.5,
            lng: 92.5,
            intensity: 0.3
        }
    ]);
</script>