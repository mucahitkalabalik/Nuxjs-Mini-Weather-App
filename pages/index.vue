<template>
  <v-row class="d-flex align-center justify-center bgcolor">
    <v-col lg="5" md="5" class="d-flex align-center justify-center">
      <v-card
        min-height="500"
        min-width="100%"
        class="d-flex align-center justify-center rounded-xl"
      >
        <v-img
          src="https://images.unsplash.com/photo-1630260643564-7f9c9c140682?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80"
          class="d-flex align-center justify-center card-img2 "
          max-height="500"
          
        >
          <v-text-field
            v-model="city"
            class="elevation-14 rounded-xl mx-5 pl-5 pt-7 mb-10 "
            color="yellow darken-4 "
            label="Şehir Adını Girin.."
            
          >
          </v-text-field>
          <v-btn @click="getWeather" class="ml-5 mr-5 elevation-14" outlined
            >Güncelle</v-btn
          >
        </v-img>
      </v-card>
    </v-col>
    <v-col lg="5" md="5" class="d-flex align-center justify-center mr-15">
      <v-card
        v-if="weather"
        min-height="500"
        min-width="100%"
        class="d-flex align-center justify-center flex-column rounded-xl"
      >
        <v-img class="card-img1" :src="selectedimages || 'https://images.unsplash.com/photo-1630260667842-830a17d12ec9?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80'" max-height="500" min-width="100%">
          <div class="d-flex align-center justify-center flex-column container-card">
            <v-card-title class="white--text"> {{ weather.name }} </v-card-title>
            <v-card-subtitle class="white--text">
              Hava Durumu : {{ weather.weather[0].description }}
            </v-card-subtitle>
            <v-card-subtitle class="white--text">
              Sıcaklık : {{ Math.ceil(weather.main.temp) }} 'C
            </v-card-subtitle>
            <v-card-subtitle class="white--text">
              Maksimum Sıcaklık : {{ Math.ceil(weather.main.temp_max) }} 'C
            </v-card-subtitle>
            <v-card-subtitle class="white--text">
              Minimum Sıcaklık : {{ Math.ceil(weather.main.temp_min) }} 'C
            </v-card-subtitle>
          </div>
        </v-img>
      </v-card>
      <v-card
        v-else
        min-height="500"
        min-width="100%"
        class="d-flex align-center justify-center flex-column"
      >
        <v-card-title> Hatalı Şehir Girdiniz.. </v-card-title>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      city: "londra",
      weather: null,
      appid: "d6be614ecc40781bd4e51ce77440004d",
      units: "metric",
      lang: "tr",
      images: {
        sunny:
          "https://images.unsplash.com/photo-1604228741406-3faa38f4907a?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=882&q=80",
        rainy:
          "https://images.unsplash.com/photo-1573553557077-4cda64089870?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80",
        cloudy:
          "https://images.unsplash.com/photo-1542549808-283c61ce5b0c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1471&q=80",
        snowy:
          "https://images.unsplash.com/photo-1635972904366-3fb9ac369ca0?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1467&q=80",
        smoke:
          "https://images.unsplash.com/photo-1635061285225-8bffdb7f45a5?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=687&q=80",
      },
      selectedimages: null,
    };
  },
  async fetch() {
    await this.loadPageGetWeather();
  },
  methods: {
    async loadPageGetWeather() {
      return new Promise((resolve, reject) => {
        console.log(this.city);
        axios
          .get(`https://api.openweathermap.org/data/2.5/weather?`, {
            params: {
              q: this.city,
              appid: this.appid,
              units: this.units,
              lang: this.lang,
            },
          })
          .then((res) => {
            this.weather = res.data;
            console.log(this.weather);
            switch (this.weather.weather[0].description) {
              case "açık":
                this.selectedimages = this.images.sunny;
                break;
              case "kar yağışlı" && "hafif kar yağışlı":
                this.selectedimages = this.images.snowy;
                break;
              case "bulutlu" && "parçalı bulutlu" && "kapalı":
                this.selectedimages = this.images.cloudy;
                break;
              case "yağmurlu" && "hafif yağmur":
                this.selectedimages = this.images.rainy;
                break;
              case "sisli":
                this.selectedimages = this.images.smoke;
                break;
              default:
              // code block
            }
            resolve();
          })
          .catch((err) => {
            console.log(err);
          });
      });
    },
    getWeather() {
      this.weather = null;
      this.selectedimages = null;
      return new Promise((resolve, reject) => {
        axios
          .get(`https://api.openweathermap.org/data/2.5/weather?`, {
            params: {
              q: this.city,
              appid: this.appid,
              units: this.units,
              lang: this.lang,
            },
          })
          .then((res) => {
            this.weather = res.data;
            console.log(this.weather);
            switch (this.weather.weather[0].description) {
              case "açık":
                this.selectedimages = this.images.sunny;
                break;
              case "hafif kar yağışlı":
                this.selectedimages = this.images.snowy;
                break;
              case "kar yağışlı":
                this.selectedimages = this.images.snowy;
                break;
              case "bulutlu" && "parçalı bulutlu" && "kapalı" && "az bulutlu":
                this.selectedimages = this.images.cloudy;
                break;
              case "yağmurlu":
                this.selectedimages = this.images.rainy;
                break;
              case "sisli":
                this.selectedimages = this.images.smoke;
                break;
              default:
            }
            resolve();
          })
          .catch(() => {});
      });
    },
  },
};
</script>

<style>
.bgcolor{
  background-color: rgba(141, 141, 141, 0.432);
}
.theme--dark.v-input input, .theme--dark.v-input textarea {
    color: rgb(233, 124, 0);
}
.theme--dark.v-label {
    color: rgb(233, 124, 0);
}
.container-card{
  background-color:rgba(0, 0, 0, 0.637);
  min-width: 100%;
  min-height: 100%;
  margin: auto;
}
.card-img1{
box-shadow: rgba(255, 255, 255, 0.4) 5px 5px, rgba(255, 255, 255, 0.3) 10px 10px, rgba(226, 226, 226, 0.2) 15px 15px, rgba(255, 255, 255, 0.1) 20px 20px, rgba(255, 255, 255, 0.05) 25px 25px;
}
.card-img2{
box-shadow: rgba(255, 255, 255, 0.4) -5px 5px, rgba(255, 255, 255, 0.3) -10px 10px, rgba(255, 255, 255, 0.2) -15px 15px, rgba(255, 255, 255, 0.1) -20px 20px, rgba(255, 255, 255, 0.05) -25px 25px;
}

</style>
