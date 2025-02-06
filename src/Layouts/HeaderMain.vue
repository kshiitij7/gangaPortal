<template>
  <v-app-bar app color="rgb(2, 42, 56)" class="home-page-header" height="80" elevation="2">
    
    <!-- Left Logo -->
    <v-img ref="indialogo" :src="indialogo" contain max-width="80" max-height="70" />
    <v-spacer></v-spacer>
    
    <!-- ISRO Logo -->
    <v-img ref="isro" :src="isro" contain max-width="70" max-height="70" style="margin-right:130px" />
   

    <!-- Header Text with Flip Animation -->
    <div ref="headerText" class="header-container" @mouseover="handleFlip">
      <div :class="['flip-container', { 'is-flipped': isFlipped }]">
        <!-- Front side -->
        <div class="header-text front">
          <h3 style="font-size: 35px;">GEO - GANGA</h3>
          <h3 style="font-size: 25px;">Spaced Based Mapping & Monitoring of River Ganga</h3>
        </div>
        <!-- Back side -->
        <div class="header-text back">
          <h3 style="font-size: 35px;">जियो - गंगा</h3>
          <h3 style="font-size: 25px;">गंगा नदी का अंतरिक्ष आधारित मानचित्रण और निगरानी</h3>
        </div>
      </div>
    </div>

    <v-spacer></v-spacer>
    <!-- IIRS Logo -->
    <v-img ref="iirs" :src="iirs" contain max-width="60" max-height="60" style="margin-left:180px" />
    <v-spacer></v-spacer>

    <!-- NMCG Logo -->
    <v-img ref="nmcg" :src="nmcg" contain max-width="80" max-height="60" style="margin-right:5px" />
    
  </v-app-bar>
</template>

<script>
// Script remains the same
export default {
  name: "HeaderMain",
  data: () => ({
    isro: require("@/assets/img/isro.png"),
    iirs: require("@/assets/img/iirs.png"),
    nmcg: require("@/assets/img/nmcgGif.gif"),
    indialogo: require("@/assets/img/india.png"),
    isFlipped: false
  }),
  methods: {
    handleFlip() {
      this.isFlipped = !this.isFlipped;
    }
  },
  mounted() {
    const headerText = this.$refs.headerText;
    const logos = [
      this.$refs.indialogo.$el,
      this.$refs.isro.$el,
      this.$refs.iirs.$el,
      this.$refs.nmcg.$el,
    ];

    headerText.style.opacity = '0';
    headerText.style.transform = 'translateY(-20px)';
    setTimeout(() => {
      headerText.style.transition = 'all 1s ease';
      headerText.style.opacity = '1';
      headerText.style.transform = 'translateY(0)';
    }, 100);

    logos.forEach((el, index) => {
      el.style.opacity = '0';
      el.style.transform = 'translateX(-30px) rotateY(90deg)';
      setTimeout(() => {
        el.style.transition = 'all 1s ease';
        el.style.opacity = '1';
        el.style.transform = 'translateX(0) rotateY(0)';
      }, 200 * (index + 1));

      el.addEventListener("mouseenter", () => {
        el.style.transform = 'scale(1.1) rotateY(10deg)';
      });
      el.addEventListener("mouseleave", () => {
        el.style.transform = 'scale(1) rotateY(0)';
      });
    });
  },
};
</script>

<style scoped>
.header-text {
  color: wheat;
  line-height: 1.1;
  font-family: "Poppins", sans-serif;
  text-align: center;
  min-width: 500px; /* Add minimum width to maintain space */
}

.header-container {
  cursor: pointer;
  perspective: 1000px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 80px; /* Match v-app-bar height */
}

.flip-container {
  position: relative;
  transition: transform 0.8s;
  transform-style: preserve-3d;
  width: 100%;
  height: 100%;
}

.is-flipped {
  transform: rotateX(180deg);
}

.front,
.back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.back {
  transform: rotateX(180deg);
}

/* Add to prevent text wrapping */
h3 {
  white-space: nowrap;
}
</style>
