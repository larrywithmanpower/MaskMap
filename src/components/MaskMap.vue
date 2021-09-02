<template>
  <div class="mask-map relative h-screen	w-full bg-gray-500" id="mask-map"></div>
</template>

<script>
  import L from 'leaflet';

  export default {
    name: 'maskMap',
    data() {
      return {
        map: {},
        markers: [],
      };
    },
    watch: {
      currDistrictInfo(dist) {
        this.map.panTo(new L.latLng(dist.latitude, dist.longitude));
      },
      filteredStores(stores) {
        this.clearMarkers();
        stores.forEach((ele) => this.addMarker(ele));
      },
    },
    computed: {
      currDistrictInfo() {
        return this.$store.getters.currDistrictInfo;
      },
      filteredStores() {
        return this.$store.getters.filteredStores;
      },
    },
    methods: {
      addMarker(item) {
        const ICON = {
          iconUrl:
            'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-violet.png',
          shadowUrl:
            'https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png',
          iconSize: [25, 41],
          iconAnchor: [12, 41],
          popupAnchor: [1, -34],
          shadowSize: [41, 41],
        };
        const bubblingMouseEvents = true;

        const marker = L.marker(
          [item.longitude, item.latitude],
          ICON,
          bubblingMouseEvents,
          )
          .addTo(
          this.map
        ).bindPopup(`
          <h2 class="text-xl font-bold"> ${item.name} </h2>
          <p class="mb-1.5">
              <i class="fas fa-head-side-mask"></i>
              成人口罩：
              ${ item.mask_adult } 片
            </p>
            <p class="mb-1.5">
              <i class="fas fa-baby"></i>
              孩童口罩：
              ${ item.mask_child }
            </p>
        `);

        marker.markerId = item.id;
        marker.lng = item.longitude;
        marker.lat = item.latitude;
        this.markers.push(marker);
      },
      clearMarkers() {
        this.map.eachLayer((layer) => {
          if (layer instanceof L.Marker) {
            this.map.removeLayer(layer);
          }
        });
        this.markers = [];
      },
      triggerPopup(markerId) {
        const marker = this.markers.find((d) => d.markerId === markerId);

        this.map.flyTo(new L.LatLng(marker.lng, marker.lat), 16);
        marker.openPopup();
      },
    },
    mounted() {
      // map init
      this.map = L.map('mask-map', {
        center: [25.03, 121.55],
        zoom: 12,
      });

      L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
        attribution:
          '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Tiles style by <a href="https://www.hotosm.org/" target="_blank">Humanitarian OpenStreetMap Team</a> hosted by <a href="https://openstreetmap.fr/" target="_blank">OpenStreetMap France</a>',
        maxZoom: 20,
      }).addTo(this.map);
    },
  };
</script>
