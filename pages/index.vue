<template>
  <div>
    <v-container class="container">
      <h1 class="title">Weather Service</h1>
      <h4 class="subtitle">just enter the city</h4>
      <form class="d-flex" @submit.prevent="getWeather">
        <v-text-field
          v-model="city"
          label="What city's weather do you want to know?"
          class="input"
        ></v-text-field>
        <v-btn elevation="2" @click="getWeather" class="btn">Search</v-btn>
      </form>
      <div v-if="pendingState" class="table">
        <img
          :src="require(`@/assets/images/stand${randomNum}.svg`)"
          class="table-img"
        />
        <div class="table-info">
          <div>
            It's <b> {{ weather.weather[0].main.toLowerCase() }} </b> there now
          </div>
          <div>
            <b>Temparature: </b>{{ Math.floor(weather.main.temp - 273.15) }} ℃
          </div>
          <div>
            <b> Feels like: </b
            >{{ Math.floor(weather.main.feels_like - 273.15) }} ℃
          </div>
          <div>
            <b>Max temperature: </b
            >{{ Math.floor(weather.main.temp_max - 273.15) }} ℃
          </div>
          <div>
            <b> Min temperature: </b
            >{{ Math.floor(weather.main.temp_min - 273.15) }} ℃
          </div>
          <div><b>Pressure: </b>{{ weather.main.pressure }}</div>
          <div><b>Humidity: </b>{{ weather.main.humidity }}</div>
          <div><b>Wind speed: </b>{{ weather.wind.speed }} m/s</div>
        </div>
      </div>
      <div v-else class="oops">
        {{ message }}
        <img :src="require(`@/assets/images/${img}`)" />
      </div>
      <footer>made with &#128420; by Maria Gileto &#169; 2021</footer>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      code: 'e4ba5ced4a81e70d0a851982e61f5944',
      weather: {},
      city: '',
      pendingState: false,
      message: '',
      img: 'alright.svg',
      randomNum: 1,
    }
  },
  methods: {
    async getWeather() {
      if (this.city.trim().length > 0) {
        try {
          const call = await fetch(
            `https://api.openweathermap.org/data/2.5/weather?q=${
              this.city[0].toUpperCase() + this.city.slice(1)
            }&appid=e4ba5ced4a81e70d0a851982e61f5944`
          )

          if (call.status > 200) {
            this.message = 'oh no! it seems like your city does not exist..'
            this.img = 'sorry.svg'
            this.pendingState = false
          } else {
            const data = await call.json()
            this.weather = data
            this.pendingState = true
            this.randomNum = Math.trunc(Math.random() * 5 + 1)
          }
        } catch (e) {
          console.log(e)
        }
      }
    },
  },
}
</script>

<style scoped>
.container {
  max-width: 70%;
  margin: 0 auto;
  font-family: 'Arial', sans-serif;
}

.btn {
  width: 100%;
}

.title {
  text-align: center;
}
.subtitle {
  color: #aaa;
  text-align: center;
}

.table {
  display: flex;
  margin: 2rem auto;
  justify-content: space-around;
}

.table div {
  margin-bottom: 10px;
}

.oops {
  margin-top: 2rem;
  display: flex;
  flex-direction: column;
  font-size: 20px;
}
.table-info {
  width: 60%;
  font-size: 25px;
  line-height: 32px;
}
img {
  margin: 0 auto;
}
.table-img {
  width: 20%;
  margin-right: 20px;
  margin-left: 0;
}

footer {
  text-align: center;
  margin-top: 2rem;
}
@media screen and (max-width: 700px) {
  .container {
    max-width: 90%;
    font-family: 'Roboto', sans-serif;
    font-size: 15px;
  }

  .table-info {
    font-size: 16px;
    line-height: 20px;
  }
}
</style>
