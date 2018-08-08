<template>
  <div id="app">
    <div class="google-map" :id="mapName"></div>
  </div>
</template>

<script>
export default {
  name: 'MapsMarker',
  data: function () {
    return {
      mapName: this.name + "-map",
      markerCoordinates: [{
        latitude: -6.218739,
        longitude: 106.802601
      }],
      map: null,
      bounds: null
    }
  },
  mounted: function () {
    this.initMarker();
  },
  methods: {
    initMarker: function () {
      var app = this;
      this.bounds = new google.maps.LatLngBounds();
      const element = document.getElementById(this.mapName)
      const mapCentre = this.markerCoordinates[0]
      const options = {
        zoom: 400,
        center: new google.maps.LatLng(mapCentre.latitude, mapCentre.longitude)
      }
      this.map = new google.maps.Map(element, options);
      this.markerCoordinates.forEach((coord) => {
        const position = new google.maps.LatLng(coord.latitude, coord.longitude);
        const marker = new google.maps.Marker({ 
          position,
          map: this.map
        });
        this.map.fitBounds(this.bounds.extend(position))
        google.maps.event.addListener(this.map, 'click', function (evt) {
          app.placeMarker(evt.latLng);
        })
      });
    },
    placeMarker: function (location) {
      let marker = new google.maps.Marker({
        position: location,
        map: this.map,
        title: 'Posisi anda'
      });
    }
  },
}
</script>

<style scoped>
.google-map {
  width: 100%;
  height: 680px;
  margin: 0 auto;
  background: gray;
}
</style>