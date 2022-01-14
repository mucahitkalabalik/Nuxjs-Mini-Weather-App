<template>
  <v-row class="d-flex align-center justify-center">
    <v-col lg="5" md="5" class="d-flex align-center justify-center">
      <v-card
        min-height="500"
        min-width="100%"
        class="d-flex align-center justify-center bg2"
      >
        <v-text-field
          v-model="city"
          class="elevation-14 ml-5 pl-5 pt-7"
          label="Şehir Adını Girin.."
        >
        </v-text-field>
        <v-btn @click="getWeather" class="ml-5 mr-5 elevation-14" outlined
          >Güncelle</v-btn
        >
      </v-card>
    </v-col>
    <v-col lg="5" md="5" class="d-flex align-center justify-center mr-15">
      <v-card
        v-if="weather"
        min-height="500"
        min-width="100%"
        class="d-flex align-center justify-center flex-column bg"
      >
        <v-img :src="selectedimages" max-height="90" max-width="100"></v-img>
        <v-card-title> {{ weather.name }} </v-card-title>
        <v-card-subtitle>
          Hava Durumu : {{ weather.weather[0].description }}
        </v-card-subtitle>
        <v-card-subtitle>
          Sıcaklık : {{ Math.ceil(weather.main.temp) }}
        </v-card-subtitle>
        <v-card-subtitle>
          Maksimum Sıcaklık : {{ Math.ceil(weather.main.temp_max) }}
        </v-card-subtitle>
        <v-card-subtitle>
          Minimum Sıcaklık : {{ Math.ceil(weather.main.temp_min) }}
        </v-card-subtitle>
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
          "https://img-fotki.yandex.ru/get/370378/493212545.712/0_20c238_21b093fa_L",
        rainy:
          "https://jf-staeulalia.pt/img/other/31/collection-animated-cloud-pictures-13.png",
        cloudy:
          "https://jf-staeulalia.pt/img/other/31/collection-animated-cloud-pictures-25.png",
        snowy:
          "https://e7.pngegg.com/pngimages/711/812/png-clipart-white-clouds-illustration-cloud-snow-weather-forecasting-snowy-weather-blue-winter.png",
        smoke:"https://w7.pngwing.com/pngs/82/829/png-transparent-computer-icons-weather-fog-foggy-text-cloud-hand.png"
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
              case "bulutlu" && "parçalı bulutlu":
                this.selectedimages = this.images.cloudy;
                break;
              case "yağmurlu":
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
      this.selectedimages=null;
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
              case "bulutlu" && "parçalı bulutlu":
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
.bg {
  background-color: rgba(19, 63, 104, 0.445) !important;
}
.bg2 {
  background-color: rgba(50, 113, 172, 0.473) !important;
}
</style>
