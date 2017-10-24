<template>
  <q-layout>
    <div id="location-map">
      <h6 class="google-map-title">¿Donde entregaremos su pedido?</h6>
      <div class="google-map-marker">
        <q-icon
          name="fa-map-pin"
        />
      </div>
      <div class="gps-button" @click="getLocation()">
        <q-icon
          name="my location"
        />
      </div>
    </div>
  </q-layout>
</template>


<script>
import {
  QLayout,
  QIcon,
  QBtn,
  LocalStorage
} from 'quasar'

export default {
  name: 'Location',
  data () {
    return {}
  },
  components: {
    QLayout,
    QIcon,
    QBtn
  },
  methods: {
    updateCoords (cameraPosition) {
      LocalStorage.set('location', cameraPosition.target.lat + ',' + cameraPosition.target.lng)
    },
    getLocation () {
      var self = this
      var div = document.getElementById('location-map')
      var map = plugin.google.maps.Map.getMap(div, {
        mapType: plugin.google.maps.MapTypeId.ROADMAP,
        'styles': [{featureType:"all",elementType:"labels.text.fill",stylers:[{color:"#7c93a3"},{lightness:"-10"}]},{featureType:"administrative.country",elementType:"geometry",stylers:[{visibility:"on"}]},{featureType:"administrative.country",elementType:"geometry.stroke",stylers:[{color:"#a0a4a5"}]},{featureType:"administrative.province",elementType:"geometry.stroke",stylers:[{color:"#62838e"}]},{featureType:"landscape",elementType:"geometry.fill",stylers:[{color:"#dde3e3"}]},{featureType:"landscape.man_made",elementType:"geometry.stroke",stylers:[{color:"#3f4a51"},{weight:"0.30"}]},{featureType:"poi",elementType:"all",stylers:[{visibility:"simplified"}]},{featureType:"poi.attraction",elementType:"all",stylers:[{visibility:"on"}]},{featureType:"poi.business",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"poi.government",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"poi.park",elementType:"all",stylers:[{visibility:"on"}]},{featureType:"poi.place_of_worship",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"poi.school",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"poi.sports_complex",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"road",elementType:"all",stylers:[{saturation:"-100"},{visibility:"on"}]},{featureType:"road",elementType:"geometry.stroke",stylers:[{visibility:"on"}]},{featureType:"road.highway",elementType:"geometry.fill",stylers:[{color:"#bbcacf"}]},{featureType:"road.highway",elementType:"geometry.stroke",stylers:[{lightness:"0"},{color:"#bbcacf"},{weight:"0.50"}]},{featureType:"road.highway",elementType:"labels",stylers:[{visibility:"on"}]},{featureType:"road.highway",elementType:"labels.text",stylers:[{visibility:"on"}]},{featureType:"road.highway.controlled_access",elementType:"geometry.fill",stylers:[{color:"#ffffff"}]},{featureType:"road.highway.controlled_access",elementType:"geometry.stroke",stylers:[{color:"#a9b4b8"}]},{featureType:"road.arterial",elementType:"labels.icon",stylers:[{invert_lightness:true},{saturation:"-7"},{lightness:"3"},{gamma:"1.80"},{weight:"0.01"}]},{featureType:"transit",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"water",elementType:"geometry.fill",stylers:[{color:"#a3c7df"}]}]
      })

      map.one(plugin.google.maps.event.MAP_READY, function() {
        // alert('--> map_canvas1 : ready.')

        var onSuccessLocation = function (location) {
          var msg = ['Current your location:\n',
            'latitude:' + location.latLng.lat,
            'longitude:' + location.latLng.lng,
            'speed:' + location.speed,
            'time:' + location.time,
            'bearing:' + location.bearing].join("\n")

          LocalStorage.set('location', location.latLng.lat + ',' + location.latLng.lng)

          map.animateCamera({
            target: location.latLng,
            zoom: 16
          }, function () {
            // alert(LocalStorage.get.item('location'))
          })
        }

        var onErrorLocation = function (msg) {
          // Set default location
          LocalStorage.set('location', '19.799697,-99.874103')

          map.animateCamera({
            target: {lat: 19.799697, lng: -99.874103},
            zoom: 16
          }, function () {
            // alert(LocalStorage.get.item('location'))
          })
        }

        // Move to the position with animation
        map.getMyLocation(onSuccessLocation, onErrorLocation)

        map.on(plugin.google.maps.event.CAMERA_MOVE_END, self.updateCoords)
      })
    }
  },
  mounted () {
    var self = this
    var div = document.getElementById('location-map')
    var map = plugin.google.maps.Map.getMap(div, {
      mapType: plugin.google.maps.MapTypeId.ROADMAP,
      'styles': [{featureType:"all",elementType:"labels.text.fill",stylers:[{color:"#7c93a3"},{lightness:"-10"}]},{featureType:"administrative.country",elementType:"geometry",stylers:[{visibility:"on"}]},{featureType:"administrative.country",elementType:"geometry.stroke",stylers:[{color:"#a0a4a5"}]},{featureType:"administrative.province",elementType:"geometry.stroke",stylers:[{color:"#62838e"}]},{featureType:"landscape",elementType:"geometry.fill",stylers:[{color:"#dde3e3"}]},{featureType:"landscape.man_made",elementType:"geometry.stroke",stylers:[{color:"#3f4a51"},{weight:"0.30"}]},{featureType:"poi",elementType:"all",stylers:[{visibility:"simplified"}]},{featureType:"poi.attraction",elementType:"all",stylers:[{visibility:"on"}]},{featureType:"poi.business",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"poi.government",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"poi.park",elementType:"all",stylers:[{visibility:"on"}]},{featureType:"poi.place_of_worship",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"poi.school",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"poi.sports_complex",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"road",elementType:"all",stylers:[{saturation:"-100"},{visibility:"on"}]},{featureType:"road",elementType:"geometry.stroke",stylers:[{visibility:"on"}]},{featureType:"road.highway",elementType:"geometry.fill",stylers:[{color:"#bbcacf"}]},{featureType:"road.highway",elementType:"geometry.stroke",stylers:[{lightness:"0"},{color:"#bbcacf"},{weight:"0.50"}]},{featureType:"road.highway",elementType:"labels",stylers:[{visibility:"on"}]},{featureType:"road.highway",elementType:"labels.text",stylers:[{visibility:"on"}]},{featureType:"road.highway.controlled_access",elementType:"geometry.fill",stylers:[{color:"#ffffff"}]},{featureType:"road.highway.controlled_access",elementType:"geometry.stroke",stylers:[{color:"#a9b4b8"}]},{featureType:"road.arterial",elementType:"labels.icon",stylers:[{invert_lightness:true},{saturation:"-7"},{lightness:"3"},{gamma:"1.80"},{weight:"0.01"}]},{featureType:"transit",elementType:"all",stylers:[{visibility:"off"}]},{featureType:"water",elementType:"geometry.fill",stylers:[{color:"#a3c7df"}]}]
    })

    map.one(plugin.google.maps.event.MAP_READY, function() {
      // alert('--> map_canvas1 : ready.')

      var onSuccessLocation = function (location) {
        var msg = ['Current your location:\n',
          'latitude:' + location.latLng.lat,
          'longitude:' + location.latLng.lng,
          'speed:' + location.speed,
          'time:' + location.time,
          'bearing:' + location.bearing].join("\n")

        LocalStorage.set('location', location.latLng.lat + ',' + location.latLng.lng)

        map.animateCamera({
          target: location.latLng,
          zoom: 16
        }, function () {
          // alert(LocalStorage.get.item('location'))
        })
      }

      var onErrorLocation = function (msg) {
        // Set default location
        LocalStorage.set('location', '19.799697,-99.874103')

        map.animateCamera({
          target: {lat: 19.799697, lng: -99.874103},
          zoom: 16
        }, function () {
          // alert(LocalStorage.get.item('location'))
        })
      }

      // Move to the position with animation
      map.getMyLocation(onSuccessLocation, onErrorLocation)

      map.on(plugin.google.maps.event.CAMERA_MOVE_END, self.updateCoords)
    })
  }
}
</script>


<style lang="css">
  #location-map {
    position: absolute;
    width: 100vw;
    height: 100vh;
    background: transparent;
  }

  .google-map-title {
    position: absolute;
    top: 0px;
    width: 100%;
    height: 50px;
    line-height: 50px;
    background-color: #fff;
    border-bottom: 1px solid #ccc;
    text-align: center;
    margin-top: 0px;
    font-size: 18px;
    box-shadow: 0px 5px 5px #ccc;
  }

  .google-map-marker {
    position: absolute;
    top: 50%;
    left: 50%;
    z-index: 2;
    margin-left: -15px;
    margin-top: -30px;
    height: 30px;
    width: 30px;
    font-size: 25px;
    color: #333;
    text-align: center;
  }

  .vue-map-hidden {
    display: inherit !important;
  }

  .gps-button {
    position: absolute;
    right: 10px;
    font-size: 26px;
    color: #333;
    background-color: rgba(255,255,255,0.8);
    border-radius: 10px;
    border: 1px solid #ccc;
    box-shadow: 5px 5px 5px #ccc;
    padding: 5px;
    padding-top: 0;
    bottom: 60px;
  }
</style>
