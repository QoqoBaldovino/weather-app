<template>
  <div id = "appweather" :class="typeof weather.main != 'undefined' && 
  (weather.weather[0].main === 'Rain' ? (weather.main.temp > 12 ? 'rainy' : 'rainy-cold'): 
  (weather.weather[0].main === 'Clouds' ? weather.main.temp > 12 ? 'cloudy' : 'cloudy-cold' : 
  (weather.weather[0].main === 'Clear' ? weather.main.temp > 12 ? 'cleary' : 'cleary-cold' : '')) ) ">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search a country..."
        v-model = "query"
        @keypress="fetchWeather"
        >
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{Math.round(weather.main.temp)}}°C</div>
        <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: '69032519ecb5831524c62cb541411a70',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`).then(res => {
          return res.json()
        }).then(this.setResults)
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

      let day = days[d.getDay()]
      let date = d.getDate();
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  }
 
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'montserrat', sans-serif;
}

#appweather{
  background-image: url('./assets/bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}


#appweather.rainy {
  background-image: url('./assets/rainy-bg.jpg');
}

#appweather.rainy-cold {
  background-image: url('./assets/rainy-cold-bg.jpg');
}

#appweather.cloudy {
  background-image: url('./assets/cloudy-bg.jpg');
}

#appweather.cloudy-cold {
  background-image: url('./assets/cloudy-cold-bg.jpg');
}

#appweather.cleary {
  background-image: url('./assets/clear-bg.jpg');
}

#appweather.cleary-cold {
  background-image: url('./assets/clear-cold-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
}

.search-box{
  width: 100%;
  margin-bottom: 100px;
}

.search-box input{
 padding: 20px;
}

.search-box, .search-bar{
  display: block;
  width: 100%;
  height: 50px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}



.search-box, .search-bar:focus{
 /* box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255, 0.75);
  border-radius: 16px 0px 16px 0px;*/
}


.weather-wrap{
  
}


.location-box .location{
  color: #FFF;
  background-color: black;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: #FFF;
  background-color: black;
  font-size: 32px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}

 .temperature{
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(97, 96, 96, 0.652);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color: #fff;
  background-color: black;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

@media only screen and (min-width: 1024px) {

  #app{
    background-color: black;
  }

  #appweather{
   
    margin: 0 auto;
    width: 30%;
  }
 
}

</style>
