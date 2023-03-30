<template>
    <div id="background" class="h-screen bg-gradient-to-br ">
     <div class="max-w-screen-lg px-4 mx-auto">

                                    
      <header class="flex items-center justify-between py-8">
          <div class="flex items-center">
            <h1 class="ml-2 text-2xl font-bold text-blue-300 font-display">Weather App</h1>
          </div>
          <div class="flex items-center">
            <div class="flex items-center">
              <img src="../assets/images/logo.png" alt="Profile" class="w-8 h-8 rounded-full">
              <span class="ml-2 font-medium text-gray-500 font-display">Emir tetik</span>
            </div>
          </div>
        </header>

                                   
<div class="flex items-center justify-center py-6 text-xl " >
                          
                          <!-- ---SEARCH--- -->

  <div class="flex flex-row items-center justify-center w-3/4 " >
    
    <div class="flex justify-center mt-6 mb-6">
      <form @submit.prevent="getWeather">

<input  type="text" placeholder="Search for city..." v-model="city" 
class="w-full p-2 text-xl font-light capitalize shadow-xl focus:outline-none placeholder:lowercase font-display">
</form><i id="#icon" class="fa-solid fa-magnifying-glass " style="font-size: 35px;  margin: 0.4rem;  "></i>
</div>
</div>

</div>
                       <!-- ---BODY--- -->
 
<div id="container" class="grid grid-cols-1 gap-4 p-6 bg-white rounded-md shadow-md md:grid-cols-3">

                         <!-- --CONTAİNER1--- -->
  <div id="container1" class="relative flex items-start w-auto p-4 my-6 rounded-md shadow-inner h-96" >

    <div class="text-xl text-white">
      
      <h1 class="flex items-center justify-center mb-2 text-xl font-medium font-display " >
        <i id="icon" class="mr-2 fa-solid fa-location-dot"></i>  {{ weather.name }},{{ weather.sys.country }}

      </h1>
      <h2 >{{ dateBuilder() }}</h2>

    </div>
  <div class="absolute bottom-10 left-10 font-display">
      <img class="w-auto h-auto rounded-full" :src="icon" alt="weather icon" >
       <p>
        <span class="text-2xl font-bold" >{{ temp() }} &deg;C <i id="icon" class="ml-2 fa-solid fa-temperature-half"></i> </span> 
       </p>
      
      <span class="text-white uppercase">{{ weather.weather[0].description }}</span>
    </div>
  </div> 

                         <!-- --CONTAİNER2--- -->
                                                                          

      <div id="container2" class="grid justify-center w-auto py-4 my-6 rounded-md col-end h-96 font-display">
              <div class="mb-2">
                <p class="flex justify-center font-bold text-blue-900"><i id="icon" class="pr-2 fa-solid fa-feather"></i>Feels like</p>
                <p class="flex justify-center text-xl font-bold"> {{Math.round(weather.main.feels_like - 273)}} &deg; C</p>
              </div>
              <div class="mb-4">
                <p class="flex justify-center font-bold text-blue-900 "><i id="icon"  class="pr-2 fa-solid fa-droplet-slash"></i>Humidity </p>
                <p class="flex justify-center text-xl font-bold"> {{ weather.main.humidity }} %</p>
              </div>
              
              <div class="mb-4">
                <p class="flex justify-center font-bold text-blue-900"><i  id="icon" class="pr-2 fa-solid fa-wind"></i> Wind  </p>
                <p class="flex justify-center text-lg font-bold">{{ weather.wind.speed }} m/s ({{ weather.wind.deg }} &deg;)</p>
        
              </div>
             
              <div class="mb-4">
                <p class="flex justify-center font-bold text-blue-900"><i id="icon" class="pr-2 fa-sharp fa-solid fa-temperature-arrow-up"></i>Max Temprature  </p>
                <p class="flex justify-center text-xl font-bold">  {{ Math.round(weather.main.temp_max - 273) }} &deg; C</p>
              </div>
              <div class="mb-0">
                <p class="flex justify-center font-bold text-blue-900"><i id="icon" class="pr-2 fa-sharp fa-solid fa-temperature-arrow-down"></i> Min Temprature </p>
                <p class="flex justify-center text-xl font-bold">  {{ Math.round(weather.main.temp_min - 273) }} &deg; C</p>
              </div>
             
            </div> 

                         <!-- --CONTAİNER3--- -->
                             
                                         
            <div id="container3" class="grid justify-center w-auto py-4 my-6 rounded-md font-display col-center h-96">
<h2 class="flex items-center justify-center mb-2 text-xl font-medium">Weekly Forecast</h2>

<ul class="flex justify-center m-0.5 " v-for="data in weeklyForecast" :key="data" >
  <li  class="relative mr-2 bg-blue-300 ">
    <span >{{ data. date }}</span>
  </li >
  <li class="mt-2 ">
    <span>{{ data.weather }}</span>
  </li>
  <li class="">
    <span class="mt-2 ml-2">{{ data.temperature }}°C</span>

  </li>
</ul> 
            </div>
            <div ><i id="icon" class="mr-2 fa-solid fa-sun"></i> {{Unix_timestamp(weather.sys.sunrise)}}</div>
            <div ><i id="icon" class="mr-2 fa-solid fa-moon"></i> {{Unix_timestamp(weather.sys.sunset)}}</div>
  </div>
    


  </div>

</div>
</template>
      


<script>
  import axios from 'axios';

export default {
   head(){
   return{
    title:"Weather App"
   }
   },
  data(){
    return{
      city:"",
      weather: {},
      weeklyForecast: null,
      
    }
  },
  
  computed:{
  icon(){ 
     return this.weather.weather ? `http://openweathermap.org/img/wn/${this.weather.weather[0].icon}.png` : ""
  }
  },
  
  mounted() {
      this.getWeeklyForecast();
    },
  asyncData({params, $axios}){
    
      return $axios      

      .$get(
        `https://api.openweathermap.org/data/2.5/weather?q=Istanbul&appid=${process.env.weatherAppId}`)

      .then(res => {

        return {weather: res}

      })
  },
  
  methods: {

     getWeather(){

      this.$axios.

      $get(
        `https://api.openweathermap.org/data/2.5/weather?q=
      ${this.city}

      &appid=${process.env.weatherAppId}`)

.then(res => (this.weather = res))
     },

  async getWeeklyForecast() {
        try {
          const response = await axios.get(
            `https://api.openweathermap.org/data/2.5/forecast?q=Istanbul,tr&units=metric&appid=${process.env.weatherAppId}`
          );
          const forecastData = response.data.list
            .filter((data) => data.dt_txt.includes('12:00:00'))
            .map((data) => ({
              date: new Date(data.dt * 1000).toLocaleDateString(),
              weather: data.weather[0].description,
              temperature: Math.round(data.main.temp),
            }));
          this.weeklyForecast = forecastData;
        } catch (error) {
          console.error(error);
        }
      },
     dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`; 
    },
    Unix_timestamp(t){
        let dt = new Date(t*1000);
        let hr = dt.getHours();
        let m = "0" + dt.getMinutes();
        let s = "0" + dt.getSeconds();
        return hr+ ':' + m.substr(-2) + ':' + s.substr(-2);  
      },
     temp(){

    return this.weather.main ? Math.round(this.weather.main.temp - 273) : ''
  },
 

  // async getWeather() {
  //   try {
  //     const response = await axios(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${process.env.weatherAppId}`);
  //     const data = await response.json();
  //     this.weather = data;
  //     this.getWeeklyForecast();
  //   } catch (error) {
  //     console.log(error);
  //   }
  // },
  // async getWeeklyForecast() {
  //   try {
  //     const response = await axios(`https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${process.env.weatherAppId}`);
  //     const data = await response.json();
  //     this.weeklyForecast = [];
  //     for (let i = 0; i < data.list.length; i += 8) {
  //       this.weeklyForecast.push({
  //         date: new Date(data.list[i].dt_txt).toLocaleDateString(),
  //         weather: data.list[i].weather[0].description,
  //       });
  //     }
  //   } catch (error) {
  //     console.log(error);
  //   }
  // },
 
  
  },
  
    
}

</script>
<style>
.font-display{
  font-family: 'Merriweather', serif;
}
ul {
	list-style-type: none;
	padding: 0;
	-webkit-box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
	        box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
	border-radius: 10px;
}
li {
	-webkit-transition: 200ms ease;
	-o-transition: 200ms ease;
	transition: 200ms ease;
	border-radius: 10px;
  width: 80px;
  word-wrap:break-word;
 
}

#container{
  background-image: url('https://img.freepik.com/free-vector/realistic-clouds-abstract-composition-with-overcast-gray-clouds-sky_1284-33932.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50% 0;
	

}

#container1{
  background-image: url('https://img.freepik.com/free-vector/realistic-clouds-abstract-composition-with-overcast-gray-clouds-sky_1284-33932.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50% 0;
	cursor: pointer;
  transition: 1s ease;

}
#container2{
 
	cursor: pointer;
  transition: 1s ease;

}
#container3{
 
	cursor: pointer;
  transition: 1s ease;

}
#container1:hover{
  -webkit-transform: scale(1.2);
-ms-transform: scale(1.2);
transform: scale(1.1);
transition: 1s ease;
}
#container2:hover{
  -webkit-transform: scale(1.2);
-ms-transform: scale(1.2);
transform: scale(1.1);
transition: 1s ease;
}
#container3:hover{
  -webkit-transform: scale(1.2);
-ms-transform: scale(1.2);
transform: scale(1.1);
transition: 1s ease;
}
#background{
  background-image: url('https://w0.peakpx.com/wallpaper/57/159/HD-wallpaper-blue-sky-clear-sky-clean-weather-peace-space-plain-background.jpg');
  background-size: cover;

}

#icon{
  font-size: 30px;
  color: #002B5B;
   
}
</style>