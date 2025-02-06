<template>
    <div class="WaterQuality">
      <v-main>
        <Globe3D style="height: 85vh;"/>
        <LeftSideBar />
        <div :class="{ showMap: showMap }" class="map-container"
        >
          <MapComponent :center="[82.0662, 26.2648]" :zoom="10" />
        </div>
        <RightSideBar />
      </v-main>
    </div>
  </template>
  
  <script>
  import MapComponent from '@/components/MapComponent.vue';
  import Globe3D from '@/Layouts/Globe3D.vue';
  import LeftSideBar from '@/Layouts/LeftSideBar.vue';
  import RightSideBar from '@/Layouts/RightSideBar.vue';
  
  export default {
    name: 'WaterQuality',
    components: {
      MapComponent,
      RightSideBar,
      LeftSideBar,
      Globe3D,
    },
    data() {
      return {
        showMap: false, // Initially hidden
      };
    },
    mounted() {
      setTimeout(() => {
        this.showMap = true;
         // Make the map visible
      }, 2500);
    },
  };
  </script>
  
  <style scoped>
  .map-container {
    height: 85vh;
    opacity: 0;
    transition: opacity 0.5s ease-in-out;
  }
  .showMap {
    opacity: 1;
  }
  </style>
  
