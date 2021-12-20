<template>
  <div class="map" ref="mapRef" style="width=100%; height: 80vh"></div>
</template>
<script>
/*.eslint-disable.no-indef.*/
import { computed, onMounted, ref } from "vue";
import { Loader } from "@googlemaps/js-api-loader";

//Dummy data
const locations = [
  ["Manly Beach", -33.80010128657071, 151.28747820854187, 2],
  ["Bondi Beach", -33.890542, 151.274856, 4],
  ["Coogee Beach", -33.923036, 151.259052, 5],
  ["Maroubra Beach", -33.950198, 151.259302, 1],
  ["Cronulla Beach", -34.028249, 151.157507, 3],
];

export default {
  name: "Map",
  setup() {
    const mapRef = ref(null);

    const loader = new Loader({
      apiKey: "AIzaSyBbfLKsQjWdalGH2i6Eqn5RoIBW_dn5fJo",
    });

    onMounted(async () => {
      const google = await loader.load();
      const directionServices = new google.maps.DirectionsService();
      const directionsDisplay = new google.maps.DirectionsRenderer();
      const infoWindow = new google.maps.InfoWindow();
      let request = { travelMode: google.maps.TravelMode.DRIVING };
      let marker, i;

      new google.maps.Map(mapRef.value, {
        center: { lat: -33.92, lng: 151.25 },
        zoom: 4,
      });

      for (i = 0; i < locations.length; i++) {
        marker = new google.maps.Marker({
          position: new google.maps.LatLng(locations[i][1], locations[i][2]),
        });

        google.maps.event.addListener(
          marker,
          "click",
          (function (marker, i) {
            return function () {
              infoWindow.setContent(locations[i][0]);
              infoWindow.open(mapRef, marker);
            };
          })(marker, i)
        );

        if (i == 0) request.origin = marker.getPosition();
        else if (i == locations.length - 1)
          request.destination = marker.getPosition();
        else {
          if (!request.waypoints) request.waypoints = [];
          request.waypoints.push({
            location: marker.getPosition(),
            stopover: true,
          });
        }
      }

      directionServices.route(request, function (result, status) {
        if (status === google.maps.DirectionsStatus.OK) {
          directionsDisplay.setDirections(result);
        }
      });
    });

    return { mapRef };
  },
};
</script>