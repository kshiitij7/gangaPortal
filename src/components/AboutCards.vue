<template>
  
    <v-container class="container" fluid >
      <v-row justify="center">
        <v-col v-for="(card, index) in cards" :key="index" cols="12" sm="6" md="3">
          <div @click="flipCard(index)" class="card-wrapper" :class="{ flipped: card.flipped }">
            <div class="card-inner">
              <!-- Front Side with Hover Effect -->
              <v-hover v-slot:default="{ props }">
                <v-card v-bind="props" class="card-wrap card-front">
                  <v-img v-if="card.image" :src="card.image" class="card-bg"></v-img>
                  <v-card-title class="card-info">
                    <div>
                      <h1><v-icon>{{ card.icon }}</v-icon> {{ card.title }}</h1>
                      <p>Click to Read More</p>
                    </div>
                  </v-card-title>
                </v-card>
              </v-hover>

              <!-- Back Side -->
              <v-card class="card-wrap card-back">
                <v-card-text class="card-back-info">
                  <p>{{ card.details }}</p>
                </v-card-text>
              </v-card>
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>
 
</template>

<script>
export default {
  data() {
    return {
      cards: [
        {
          icon: "mdi-leaf",
          title: "Ganga Basin",
          details: "The Ganga Basin is a crucial water source for millions.",
          image: require('@/assets/img/about_img.png'),
          flipped: false,
        },
        {
          icon: "mdi-water-check",
          title: "Water Quality",         
          details: "Water quality monitoring ensures clean and safe water.",
          image: require('@/assets/img/waterquality.png'),
          flipped: false,
        },
        {
          icon: "mdi-alert",
          title: "Flood Hazard",
          details: "Flood hazards impact infrastructure and ecosystems.",
          image: "https://www.mdpi.com/sustainability/sustainability-14-01472/article_deploy/html/images/sustainability-14-01472-g001-550.jpg",
          flipped: false,
        },
        {
          icon: "mdi-recycle",
          title: "Solid Waste",
          details: "Proper waste management reduces environmental pollution.",
          image: "https://www.researchgate.net/publication/338333116/figure/fig1/AS:961353997103113@1606216062796/Map-of-the-Ganga-River-Basin-showing-land-use-and-sampling-locations-for-the-collection.png",
          flipped: false,
        },
      ],
    };
  },
  methods: {
    flipCard(index) {
      this.cards[index].flipped = !this.cards[index].flipped;
    },
  },
};
</script>

<style scoped>
.container {
  padding: 50px 50px;
  display: flex;
  flex-wrap: wrap;
  background-size: cover;
  background-position: center;
}

.card-wrapper {
  perspective: 1000px;
  width: 80%;
  height: 350px;
  cursor: pointer;
}

.card-inner {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.6s ease-in-out;
}

.flipped .card-inner {
  transform: rotateY(180deg);
}

.card-wrap {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.1);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15), 0 4px 6px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  overflow: hidden;
  backface-visibility: hidden;
}

/* Front Card Styling */
.card-front {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}

.card-bg {
  opacity: 0.9;
  transition: opacity 0.6s ease-out;
  border-radius: 12px;
}

.card-info {
  position: absolute;
  bottom: 0;
  color: #fff;
  transform: translateY(30%);
  transition: transform 0.6s ease-out, opacity 0.6s ease-out;
  padding: 10px;
  background-color: rgba(0, 0, 0, 0.6);
  border-top-left-radius: 12px;
  border-top-right-radius: 12px;
}

.card-info h1 {
  font-family: "Poppins", sans-serif;
  color: whitesmoke;
  font-size: 20px;
  font-weight: 700;
}

.card-info p {
  font-family: "Poppins", sans-serif;
  font-size: 14px;
  opacity: 0;
  color: white;
  text-decoration: underline;
  text-shadow: rgba(black, 1) 0 2px 3px;
}

.card-wrap:hover .card-info {
  transform: translateY(0);
}

.card-wrap:hover .card-info p {
  opacity: 1;
}

/* Back Card Styling */
.card-back {
  transform: rotateY(180deg);
  background: rgba(0, 0, 0, 0.9);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: white;
  padding: 20px;
}
.card-back-info p {
  font-size: 14px;
  color: #ddd;
  margin: 5px;
  font-family: "Poppins", sans-serif;
  text-align: justify;
}
</style>
