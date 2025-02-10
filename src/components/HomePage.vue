<template>
  <v-main>
    <div class="image-container">
      <v-img :src="homeImage" class="image" />
      <div class="overlay-text animate-text">
        Geo-Ganga
      </div>
      <AboutCards style="position:absolute; z-index:2; top: 65%;" />
    </div>    
    <FooterHome style="bottom: 0;"/>
  </v-main>
      
    </template>
    
  
  <script>
  import FooterHome from '@/Layouts/FooterHome.vue';
  import AboutCards from './AboutCards.vue';
    
  
  export default {
      name: 'HomePage',
      components: {
          FooterHome,
          AboutCards,
      },
      data() {
          return {
              homeImage: require('@/assets/HomeImages/1.jpg'),
          };
      },
  };
  </script>
  
  <style scoped>
  @keyframes fadeSlideIn {
    0% {
      opacity: 0;
      transform: translate(-50%, -100%);
    }
    100% {
      opacity: 1;
      transform: translate(-50%, -50%);
    }
  }
  .overlay-text {
    width: 100%;
    position: absolute;
    top: 15%;
    left: 70%;
    transform: translate(-50%, -50%);
    font-size: 150px;
    font-weight: bold;
    text-align: center;
    font-family: "Poppins", sans-serif;
    opacity: 0;
    background: linear-gradient(45deg, #3f857e, #211b75, #004d40, #0288d1, #ff5722);
    -webkit-background-clip: text;
    color: transparent;
    animation: fadeSlideIn 1.5s ease-out forwards;
    letter-spacing: 2px;
    text-transform: uppercase;
  }
  .animate-text {
    animation: fadeSlideIn 1.5s ease-out forwards;
  }
  .image-container {
    position: relative;
    margin: 0 auto;
    max-width: 100%;
    height: auto;
  }
  </style>
  
