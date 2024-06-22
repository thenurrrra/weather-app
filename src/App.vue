<script>
import axios from 'axios'

  export default {
    data() {
      return {
        city: "",
        error: "",
        info: null,
      }
    },
    computed: {
      showTemp() {
        return this.info.main.temp
      },
      showFeelsLike() {
        return this.info.main.feels_like
      },
      showMaxTemp() {
        return this.info.main.temp_max
      },
      showMinTemp() {
        return this.info.main.temp_min
      }
    },
    methods: {
      getWeather() {
        if (this.city.trim().length < 2) {
          this.error = "Нужно название более одного символа!"
          return false
        }

        this.error = ""

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=c7f4dc3b643f08e1732cfc419e322332`)
          .then(res => (this.info = res.data))
      },
    }
  }
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : city }}</p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-if="city != ''" @click="getWeather()">Узнать погоду</button>
    <button disabled v-else>Узнать погоду</button>
    <p class="error">{{ error }}</p>
    <div v-if="info != null" class="weathers">
      <p>Температура на данный момент: <br>{{ showTemp }}</p>
      <p>Ощущается как: <br>{{ showFeelsLike }}</p>
      <p>Максимальная температура: <br>{{ showMaxTemp }}</p>
      <p>Минимальная температура: <br>{{ showMinTemp }}</p>
    </div>
  </div>
</template>

<style scoped>

.wrapper .weathers {
  display: flex;
}



.wrapper .error {
  color: #561c1c;
  font-weight: 600;
  margin-top: 30px;
}

.wrapper {
  height: 370px;
  width: 700px;
  padding: 50px;
  text-align: center;
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 50px;
  transition: .2s linear;
}


.wrapper h1 {
  color: #fff;
  margin-top: 50px;
}

.wrapper p {
  color: #fff;
  margin-top: 20px;
  margin-bottom: 50px;
}

.wrapper input {
  border: none;
  width: 100px;
  border-radius: 10px;
  background-color: transparent;
  color: #fff;
  padding: 10px 20px;
  transition: .3s ease-in-out;
  margin-right: 20px;
}

.wrapper input:hover {
  border-left: none;
  box-shadow: 0 0 20px rgb(84, 18, 84);
}

.wrapper input:focus {
  outline: none;
  box-shadow: 0 0 20px rgb(84, 18, 84);
}

.wrapper button {
  color: #fff;
  background-color: rgb(84, 18, 84);
  border: none;
  border-radius: 10px;
  padding: 10px 15px;
  cursor: pointer;
  box-shadow: 0 0 10px rgb(84, 18, 84);
  transition: .3s linear;
}

.wrapper button:hover {
  box-shadow: 0 0 20px rgb(84, 18, 84);
}

.wrapper button:disabled {
  box-shadow: none;
  background-color: rgb(43, 9, 43);
  color: #3e3e3e;
  cursor: not-allowed;
}

</style>
