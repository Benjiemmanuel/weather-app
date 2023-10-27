<template>
  <div class="weather-app" :class="backgroundClass">
    <div class="container">
      <div class="weather-content">
        <form class="search-location" @submit.prevent="getWeather">
          <input type="text" class="search-bar" v-model="citySearch" autocomplete="off" placeholder="Enter City">
        </form>
        <div><h3>Feels Like {{weather.feelsLike}}</h3></div>
        <div class="Weather-logo">
          <img class="logo" src='./assets/cloudpic.png' alt="Logo">
          <div class="Temperature-reading">
            <div class="temp-read"><h2>{{weather.temperature}}&deg;</h2></div>
          </div>
        </div>
        <div><h1>{{weather.description}}</h1> </div>
        <div class="line-cross"></div>
        <div class="weather-reading">
          <span>{{weather.wind}}</span>
          <span>{{weather.humidity}}</span>
          <span>{{weather.rain}}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import cityWeatherData from '@/assets/cityWeatherData.json';

export default {
  data() {
    return {
      cityData: cityWeatherData.cities,
      citySearch: "",
      weather: {
        cityName: "",
        temperature: "24",
        humidity: "34%",
        rain: "55%",
        wind: "44",
        feelsLike: "56",
        description: "Broken Clouds"
      }
    };
  },
  methods: {
    getWeather() {
      const searchedCity = this.citySearch.trim().toLowerCase();
      const foundCity = this.cityData.find(city => city.name.toLowerCase() === searchedCity);
      if (foundCity) {
        this.weather.cityName = foundCity.name;
        this.weather.temperature = foundCity.weather.temp;
        this.weather.feelsLike = foundCity.weather.feelsLike;
        this.weather.description = foundCity.weather.description;
        this.weather.rain = foundCity.weather.rain;
        this.weather.humidity = foundCity.weather.humidity;
        this.weather.wind = foundCity.weather.wind;
        this.citySearch = ""; 
      } else {
        this.weather.cityName = "City Not Found";
        this.weather.temperature = "0";
        this.weather.feelsLike = "0";
        this.weather.description = "No data available";
        this.weather.rain = "0%";
        this.weather.humidity = "0%";
        this.weather.wind = "0.0";
      }

  }
  },
    computed: {
    backgroundClass() {
      const { description } = this.weather;

      if (description.toLowerCase().includes('rain')) {
        return 'rainy-background';
      } else if (description.toLowerCase().includes('cloud')) {
        return 'cloudy-background';
      } else if (description.toLowerCase().includes('mist')) {
        return 'mist-background';
      }
      else if (description.toLowerCase().includes('sun')) {
        return 'sunny-background';
      }
      else {
        return 'default-background';
      }
    },
       getLogoSrc() {
      const { description } = this.weather;
      
      if (description.toLowerCase().includes('rain')) {
        return './assets/rainy-logo.png';
      } else if (description.toLowerCase().includes('cloud')) {
        return './assets/cloudy-logo.png'; 
      } else if (description.toLowerCase().includes('mist')) {
        return './assets/mist-logo.png';
      } else if (description.toLowerCase().includes('sun')) {
        return './assets/sunny-logo.png'; 
      } else {
        return './assets/default-logo.png';
      }
    },
    
  },
};
</script>


<style>
@keyframes moveClouds {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: 100% 0;
  }
}

.weather-app {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 0;
  margin: 0;
  background-image: url('./assets/sunny.webp'); 
  background-size: cover;
  animation: moveClouds 5s linear infinite;
  transition: background-image 0.5s;
}

.rainy-background {
  background-image: url('./assets/rainy.jpg');
  animation: moveRain 5s linear infinite; 
}
.sunny-background {
  background-image: url('./assets/sunny.webp');
  animation: moveRain 5s linear infinite; 
}

.cloudy-background {
  background-image: url('./assets/cloudjpg.jpg'); 
  animation: moveClouds 5s linear infinite;
}
.mist-background {
  background-image: url('./assets/mistpng.jpg'); 
  animation: moveClouds 5s linear infinite;
}
.default-background {
  background-image: url('./assets/sunny.webp'); 
  animation: none;
}

*{
  padding: 0;
  margin: 0;
}
.container{
  background: rgb(247, 245, 245);
  box-sizing: border-box;
  width: 18%;
  height: 55vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
}
  .weather-contents {
  padding: 10px;
  text-align: center;
  border-radius: 10px;
  align-items: center;
  justify-content: center;
  display: flex;
  flex-direction: column;
} 
.search-location{
  display: flex;
  flex-direction: column;
}
h1{
        font-size: 15px;
        color:gray;
        font-family: sans-serif;
        text-align: center;
        padding-top: 20px;
        padding-bottom: 20px;
    }
   
    h3{
        font-size: 15px;
        color:gray;
        font-family: sans-serif;
        text-align: center;
        padding: 30px;
    }
      input[type="text"] {
        width: 190px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
            box-shadow:0px 0px 10px gray ;
            outline: none;
             background:transparent ;
             border-radius: 15px;
        }
     input[type="text"]:focus {
            border-color: #bbbdbe;
           box-shadow:0px 0px 10px gray ;
        }
    .Temperature-reading{
        margin-left: 40px;
        width: 130px;
        display: flex;
        align-items: center;
        justify-content: center;
        background:transparent;
        height: 130px;
        box-sizing: border-box;
        border-radius: 50%;
        box-shadow: 0px 3px 35px gray;
    }
     .logo{
      width: 75px;
      height: 65px;
      position: absolute;
      bottom: 380px;
      padding-left: 20px;
    }
    .temp-read{
      font-size: 40px;
    }
    .Weather-logo{
      display: flex;
      flex-direction: row;
    }
    .line-cross {
     width: 100%;
     height: 2px;
     background-color: rgb(235, 232, 232); 
       
    
    }
      .weather-reading{
        color: gray;
        display: flex;
        flex-direction: row;
        gap: 55px;
        padding-top: 10px;
        box-sizing: border-box;
        padding-top: 20px;
    }
     ::placeholder{
        padding-left: 20px;
    }
</style>