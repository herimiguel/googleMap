<template>
  <div class="App"/>
</template>

<script>
import MarkerClusterer from '@google/markerclusterer';

import gmapsInit from './utils/gmaps';
// import { lookup } from 'dns';
 const locations = [
  // {
  //   position: {
  //     lat: 48.160910,
  //     lng: 16.383330,
  //   },
  // },
  // {
  //   position: {
  //     lat: 48.174270,
  //     lng: 16.329620,
  //   },
  // },
  //   {
  //   position: {
  //     lat: 48.174570,
  //     lng: 16.329320,
  //   },
  // },  
  // {
  //   position: {
  //     lat: 48.175270,
  //     lng: 16.329120,
  //   },
  // },
  // ...
];

export default {
  name: 'App',
  async mounted() {
    try {
      const google = await gmapsInit();
      const geocoder = new google.maps.Geocoder();
      const map = new google.maps.Map(this.$el);


      geocoder.geocode({ address: 'Chicago' }, (results, status) => {
        if (status !== 'OK' || !results[0]) {
          throw new Error(status);
        }

        map.setCenter(results[0].geometry.location);
        map.fitBounds(results[0].geometry.viewport);
      });
      
      const markerClickHandler = (marker) => {
        map.setZoom(13);
        map.setCenter(marker.getPosition());
      };

      const markers = locations.map((location) => {
        const marker = new google.maps.Marker({ ...location, map });
        marker.addListener('click', () => markerClickHandler(marker));
        return marker;
    });

     new MarkerClusterer(map, markers, {
       imagePath: 'https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/m', 
     });
    } catch (error) {
      console.error(error);
    }
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

.App {
  width: 100vw;
  height: 100vh;
}
</style>