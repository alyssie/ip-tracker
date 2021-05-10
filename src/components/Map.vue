<template>
    <div class="map">
        <l-map style="height: 100%" :zoom="zoom" :center="center">
            <l-tile-layer :url="url"></l-tile-layer>
            <l-marker :lat-lng="markerLatLng" ></l-marker>
        </l-map>
    </div>
</template>

<script>
    import { LMap, LTileLayer, LMarker} from "vue2-leaflet";
    import { Icon } from 'leaflet';

    export default {
        name: "Map",
        props: {
            lat: Number,
            lng: Number
        },
        data () {
            return {
                url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
                zoom: 15,
                center: [this.lat, this.lng],
                markerLatLng: [this.lat, this.lng]
            };
        },
        components: {
            LMap,
            LTileLayer,
            LMarker
        },
        beforeMount(){
            delete Icon.Default.prototype._getIconUrl;
            Icon.Default.mergeOptions({
                iconRetinaUrl: require('../assets/icon-location.svg'),
                iconUrl: require('../assets/icon-location.svg'),
                shadowUrl: null,
                iconSize: [46, 56],
            });
        }
    };
</script>

<style lang="scss">
    .map {
        height: calc(100vh - 280px);
        width: 100%;
        margin: 0 auto;
        position: relative;
    }
</style>
