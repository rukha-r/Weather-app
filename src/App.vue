<template>
  <div id="app" :class='day' class="animate__animated animate__jackInTheBox">

    <main>
      <div class="weather-wrapper">
        <div class="location-search">
          <input @keypress.enter="updateWeather" type="text" class="search-area" placeholder="Search..." v-model="search">  
        </div>
        <div class="location-box">
        <h3 class="locat"> {{ currentWeather.name }} . {{ currentWeather.sys.country }} </h3>
        <div class="day-box">
        <h5 class="day">{{ currentDay }}</h5>
        </div>
       <div class="temperature-wrapper">
       <h1 class="temperature"> {{ temp }}°C </h1>
       <h4 class="weather-condition"> {{ currentWeather.weather[0].main }} </h4>
       </div>        
       </div>
      </div>
    </main>  
  </div>
</template>



<script>
export default {
  name: 'App',
  data(){
    return {
      api_key: '8485ad7ba53bf54708caa071a34301c0',
      search: '',
      base_url: 'https://api.openweathermap.org/data/2.5/',
      currentWeather: {},
      day: ''
    }
  },

  methods: {    
    updateWeather(){
      fetch(`${this.base_url}weather?q=${this.search}&units=metric&appid=${this.api_key}`).then(res => res.json()).then(this.setResults).then(this.updateDay)
        },
    setResults(results) {
          this.currentWeather = results;
          this.search = '';
        },
    updateDay(){
      if(this.currentWeather.weather[0].main === 'Rain' || this.currentWeather.weather[0].main === 'Rainy') {
       this.day = 'rainy';
     }
    else if(this.currentWeather.weather[0].main === 'Sunny' || this.currentWeather.weather[0].main === 'Warm' || this.currentWeather.weather[0].main === 'Clear' ) {
      this.day = 'sunny';
     } 
    else if(this.currentWeather.weather[0].main === 'Clouds' || this.currentWeather.weather[0].main === 'Cloudy') {
       this.day = 'cloudy';
        }
     else if(this.currentWeather.weather[0].main === 'Snow' || this.currentWeather.weather[0].main === 'Snowing') {
       this.day = 'snow';
        }
     else if(this.currentWeather.weather[0].main === 'Haze' || this.currentWeather.weather[0].main === 'Hazy') {
       this.day = 'hazy';
        }         
    }
  },

  computed: {
     currentDay(){
       let weekDays = ['Monday','Tusday','Wednesday','Thursday','Friday','Saturday','Sunday'];
       let monthList = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
       let d = new Date();
       let date = d.getDate();
       let day = d.getDay();
       let month = d.getMonth();
       let year = d.getFullYear();
       return `${weekDays[day]} / ${monthList[month]}  ${date} / ${year}`;
     },     
     temp(){
       return Math.round(this.currentWeather.main.temp);
     }
      
 },
  
  created(){
     this.search = 'New York';
     this.updateWeather();
   }
}    

</script>


<style lang="scss">
@import './assets/Styling.scss';

body{
  font-family: Arial, Helvetica, sans-serif;
  padding-top: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: $body-background-color;
 #app {
    main {
    color: #bdbdbd;
    text-align: center;
    .weather-wrapper {
      padding: 60px;
      border-radius: 20px;
      background-color: $main-background-color;
    }
    .search-area {
      width: 200px;
      padding: 12px;
      border-radius: 15px 15px 0 0;
      font-size: 15px;
      color: $search-area-text-color;
      border: none;
      background-color: $location-box-bgColor;
      text-align: center;
    }
   }
   .location-box {
     padding:10px;
     border-radius: 0 0 15px 15px;
     background-color: $location-box-bgColor;
     .temperature {
       background-color: #383737;
       color: #cecece;
       width: 70px;
       padding: 5px;
       margin: 0 0 0 60px;
       border-radius: 10px;
       border: none;
     }
   }    
  }
  .sunny {
  @include appBox;
  background-image: $sunny-day;
 }
 .cloudy {
  @include appBox;
  background-image: $cloudy-day;
 }
 .rainy {
  @include appBox;
  background-image: $rainy-day;
 }
 .snow {
   @include appBox;
  background-image: $snowing-day;
 }
 .hazy {
   @include appBox;
  background-image: $hazy-day;
 }
}


</style>
