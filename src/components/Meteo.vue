<template>
  <div
    class="body"
    :class="typeof weather.main != 'undefined' && weather.weather[0].main === 'Clear' ? 'body-clear-sky' : '' || typeof weather.main != 'undefined' && weather.weather[0].main === 'Clouds' ? 'body-clouds' : '' || typeof weather.main != 'undefined' && weather.weather[0].main === 'Rain' ? 'body-rain' : '' || typeof weather.main != 'undefined' && weather.weather[0].main === 'Snow' ? 'body-snow' : ''"
  >
    <div class="container">
      <div class="search-box">
        <input
          type="text"
          class="search-bar margin-top-10"
          @keypress="fetchWeather"
          v-model="query"
          placeholder="Search..."
        />
      </div>
      <div class="jour">
        <transition name="fade">
          <h2 v-if="this.clou" class="text-white text-light-shadow">{{ weather.name }}</h2>
        </transition>
        <transition name="fade">
          <h1 v-if="this.clou" class="text-white text-light-shadow">{{ format(now) }}</h1>
        </transition>
      </div>
      <transition name="fade">
        <div v-if="typeof weather.main != 'undefined' && this.clou">
          <div class="flex-row justify-content-center">
            <h2 class="text-white">
              <span
                class="box-shadow padding-20 backgroun-color-white border-radius-10 font-size-big text-light-shadow padding-right-20 padding-left-20"
              >{{ Math.round(weather.main.temp) }}°</span>
            </h2>
          </div>
          <!--Clouds-->
          <h2
            class="text-white no-margin-top"
            v-if="weather.weather[0].description === 'broken clouds'"
          >Fragments de nuages</h2>
          <h2
            class="text-white no-margin-top"
            v-if="weather.weather[0].description === 'light intensity drizzle'"
          >Brumeux</h2>
          <h2
            class="text-white no-margin-top"
            v-if="weather.weather[0].description === 'overcast clouds'"
          >Couvert</h2>
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'Clouds'"
          >Nuageux</h2>
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'few clouds'"
          >Quelques nuages</h2>
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'scattered clouds'"
          >Partiellement couvert</h2>
          <!--Rain-->
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'light rain'"
          >Légère pluie</h2>
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'Rain'"
          >Pluie</h2>
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'moderate rain'"
          >Pluie modérée</h2>
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'light snow'"
          >Légère neige</h2>
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'Snow'"
          >Neige</h2>
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].description === 'moderate snow'"
          >Neige modérée</h2>
          <!--Sky-->
          <h2
            class="text-white no-margin-top"
            v-else-if="weather.weather[0].main === 'Clear'"
          >Ciel clair</h2>
        </div>
      </transition>
      <transition name="fade">
        <div v-if="this.clou">
          <h2 class="text-white no-margin-bot no-margin-top">prévisions</h2>
          <div class="flex-row overflow padding-left">
            <div
              v-for="weather2Prev in weather2.list"
              :key="weather2Prev.id"
              class="margin-right no-margin-bot"
            >
              <div class="flex-column">
                <img :src=" 'https://openweathermap.org/img/wn/' + weather2Prev.weather[0].icon + '.png'"/>
                <div class="text-center text-white font-xs">{{ weather2Prev.main.temp.toFixed(0) }}°</div>
                <!--Rain-->
                <div
                  v-if="weather2Prev.weather[0].description === 'light rain'"
                  class="text-center text-white font-xs margin-top"
                >Légère pluie</div>
                <div
                  v-else-if="weather2Prev.weather[0].description === 'Rain'"
                  class="text-center text-white font-xs margin-top"
                >Pluie</div>
                <div
                  v-else-if="weather2Prev.weather[0].description === 'moderate rain'"
                  class="text-center text-white font-xs margin-top"
                >Pluie modérée</div>
                <!--Clouds-->
                <div
                  v-else-if="weather2Prev.weather[0].description === 'Clouds'"
                  class="text-center text-white font-xs margin-top"
                >Nuageux</div>
                <div
                  v-else-if="weather2Prev.weather[0].description === 'broken clouds'"
                  class="text-center text-white font-xs margin-top"
                >Fragments de nuages</div>
                <div
                  v-else-if="
                  weather2Prev.weather[0].description === 'overcast clouds'
                "
                  class="text-center text-white font-xs margin-top"
                >Couvert</div>
                <div
                  v-else-if="weather2Prev.weather[0].description === 'few clouds'"
                  class="text-center text-white font-xs margin-top"
                >Quelques nuages</div>
                <div
                  v-else-if="
                  weather2Prev.weather[0].description === 'scattered clouds'
                "
                  class="text-center text-white font-xs margin-top"
                >Partiellement couvert</div>
                <!--Sky-->
                <div
                  v-else-if="weather2Prev.weather[0].description === 'clear sky'"
                  class="text-center text-white font-xs margin-top"
                >Ciel clair</div>
                <div
                  class="text-center width text-white font-xs margin-top"
                >{{ format(weather2Prev.dt_txt) }}</div>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { formatRelative } from "date-fns";
import { fr } from "date-fns/locale";
export default {
  name: "Meteo",
  data: function() {
    return {
      clou: false,
      apiKey: "2991540a50e60f697b3519f525aa1037",
      urlBase: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      weather2: {},
      now: new Date(),
      infosjour: null
    };
  },
  mounted() {
    setInterval(() => {
      this.now = new Date();
    }, 1000);
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.urlBase}weather?q=${this.query}&units=metric&APPID=${this.apiKey}`
        )
          .then(res => {
            this.clou = true;
            this.query = "";
            return res.json();
          })
          .then(
            this.setResults,

            (this.clou = !this.clou)
          );
      }
      if (e.key == "Enter") {
        fetch(
          `${this.urlBase}forecast?q=${this.query},3003093&units=metric&APPID=${this.apiKey}`
        )
          .then(res => {
            this.query = "";
            return res.json();
          })
          .then(this.setResults2);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    setResults2(results) {
      this.weather2 = results;
    },
    format(date) {
      return formatRelative(new Date(date), this.now, { locale: fr });
    }
  },
  created() {
    axios
      .get(
        "https://api.openweathermap.org/data/2.5/forecast?q=Lens,3003093&units=metric&APPID=62ca4d47cef5a36196f8d74f12c1ae13"
      )
      .then(response => (this.infosjour = response.data));
  }
};
</script>
<style lang="css">
body {
  margin: 0;
}
.body {
  font-family: "Anton", sans-serif;
  background-size: 1020px;
  background-position: center center;
  background-image: url(../assets/mount.png),
    linear-gradient(
      to bottom,
      #3de9ff,
      #11dcff,
      #00cfff,
      #03c0ff,
      #33b1ff,
      #1ca3ff,
      #2693ff,
      #4281ff,
      #1f6dff,
      #0056ff,
      #003bff,
      #2100ff
    );
}
.body-clear-sky {
  font-family: "Anton", sans-serif;
  background-size: 1020px;
  background-position: center center;
  background-image: url(../assets/mountain-clear-sky.png),
    linear-gradient(
      to bottom,
      #3de9ff,
      #11dcff,
      #00cfff,
      #03c0ff,
      #33b1ff,
      #1ca3ff,
      #2693ff,
      #4281ff,
      #1f6dff,
      #0056ff,
      #003bff,
      #2100ff
    );
}
.body-clouds {
  font-family: "Anton", sans-serif;
  background-size: 1020px;
  background-position: center center;
  background-image: url(../assets/mountain-clouds.png),
    linear-gradient(
      to bottom,
      #929191,
      #979696,
      #9b9a9a,
      #a09f9f,
      #a5a4a4,
      #aaa9a9,
      #afaeaf,
      #b4b3b4,
      #bab9ba,
      #c0c0c1,
      #c7c6c7,
      #cdcdcd
    );
}
.body-rain {
  font-family: "Anton", sans-serif;
  background-size: 1020px;
  background-position: center center;
  background-image: url(../assets/mountain-rain.png),
    linear-gradient(
      to bottom,
      #929191,
      #979696,
      #9b9a9a,
      #a09f9f,
      #a5a4a4,
      #aaa9a9,
      #afaeaf,
      #b4b3b4,
      #bab9ba,
      #c0c0c1,
      #c7c6c7,
      #cdcdcd
    );
}
.body-snow {
  font-family: "Anton", sans-serif;
  background-size: 1020px;
  background-position: center center;
  background-image: url(../assets/mountain-snow.png),
    linear-gradient(
      to bottom,
      #ababab,
      #b0b0b0,
      #b6b6b6,
      #bbbbbb,
      #c1c1c1,
      #c6c6c6,
      #cbcbcb,
      #d0d0d0,
      #d5d5d5,
      #dbdbdb,
      #e0e0e0,
      #e6e6e6
    );
}
h1,
h2 {
  text-align: center;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  height: 100vh;
}
.flex-row {
  display: flex;
}
.flex-column {
  display: flex;
  flex-direction: column;
}
.text-center {
  text-align: center;
}
.margin-right {
  margin-right: 10px;
}
.width {
  width: 145px;
}
.overflow {
  overflow: scroll;
}
.text-white {
  color: rgb(255, 255, 255);
}
.font-xs {
  font-size: 0.8rem;
}
.margin-top-10 {
  margin-top: 10px;
}
.margin-top {
  margin-top: 5px;
}
.no-margin-bot {
  margin-bottom: 0;
}
.no-margin-top {
  margin-top: 0;
}
.padding-left {
  padding-left: 10px;
}
.search-box {
  text-align: center;
}
.search-bar {
  width: 270px;
  border-radius: 10px 0px;
  border: none;
  padding: 10px;
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
}
.text-light-shadow {
  text-shadow: 1px 1px 2px rgb(0, 0, 0);
}
.box-shadow {
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
}
.padding-20 {
  padding: 10px;
}
.backgroun-color-white {
  background-color: rgba(255, 255, 255, 0.144);
}
.border-radius-10 {
  border-radius: 10px;
}
.font-size-big {
  font-size: 5rem;
}
.justify-content-center {
  justify-content: center;
}
.padding-right-20 {
  padding-right: 20px;
}
.padding-left-20 {
  padding-left: 20px;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
