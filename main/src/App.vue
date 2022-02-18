 <template>
  <div id="app">
     <div class="main">
      <div class="search-box">
        <form v-on:submit.prevent="fetchWeather" >
          <input type="text" 
          placeholder="What city?"
          class="mysearch"
          v-model="query">
        </form>
        <p class="text-center my-3 text-danger" v-if="found"> City not found</p>
     </div>

     <div class="text-center startstyle" v-if="start">
     <h5>Discover the Weather in your city</h5>
      </div>

     <div class="weather-details" v-if="visible">
       <div class="location-details">
         <div class="location mt-2">
             {{weather.cityName}}, <span class="icon">{{weather.country}}</span>
         </div>
         <div class="date">
            {{dateBuilder()}}
         </div>
       </div>
      
        <div class="temperature">
          {{weather.temperature}}
          </div>
        <div class="weather-box">
          <div class="weather mb-5">
              <img v-bind:src="pic" alt=""> 
            {{weather.description}}
          </div>
        </div>


        <div class="mtemp px-5 py-4 row">
          <div class="col text-center">
           <span> <i class="fas fa-thermometer-quarter"></i> </span>
            <span> {{weather.lowtemp}}</span>
          </div>
          <div class="col text-center">
            <span> <i class="fas fa-thermometer-full"></i></span>
            <span> {{weather.hightemp}}</span>
          </div>
          
        </div>
    

        <div class="mtemp2 px-5 py-4 row">
         
          <div class="col text-center">
             <p>{{weather.feelslike}}</p>
            <span>feels like</span>
          </div>
          <div class="col text-center">
            <p>{{weather.humidity}}</p>
            <span>humidity</span>
          </div>
        </div>

     </div>
     <div>

     </div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data:function(){
     return{
       query:"",
       visible:false,
       found:false,
       start:true,
       weather:{
         cityName:"lagos",
         country:"Ng",
         temperature:"",
         description:""
        },
        pic:'http://openweathermap.org/img/wn/${iconName}.png'
     }
  },
  methods:{
    fetchWeather: async function(){
      console.log(this.fetchWeather);
      const key ="907db79aa6b2083d95845bdb6c6adc9b"
      const baseURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.query}&appid=${key}&units=metric`;
     
     
     try{
      const response = await fetch(baseURL);
      const data = await response.json()
      console.log(data);
      this.iconName = data.weather[0].icon;
      this.query = "";
      this.weather.cityName = data.name;
      this.weather.country = data.sys.country;
      this.weather.temperature = Math.round(data.main.temp) + "°c";
      this.weather.description = data.weather[0].description;
      this.weather.lowtemp = Math.round(data.main.temp_min) + "°c";
      this.weather.hightemp = Math.round(data.main.temp_max) + "°c";
      this.weather.feelslike = Math.round(data.main.feels_like) + "°c";
      this.weather.pressure = Math.round(data.main.pressure) + "°c";
      this.weather.humidity = Math.round(data.main.humidity) + "%";
      this.found = false;
      this.visible = true; 
      
      
      
     }catch (error){
       console.log(error);
       this.found = true;
     }

   
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  },
  
  components: {
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body{
font-family: 'montserrat', sans-serif;
}
.main{
 min-height: 100vh;
 padding: 30px;
 background-image: linear-gradient(to bottom, rgb(0 0 0 / 76%), rgb(48 43 43));
}
.search-box{
  width:100%;
  margin-bottom: 30px;
}
.mysearch{
  padding: 15px;
  display: block;
  color: rgb(39, 39, 39);
  font-size: 18px;
  font-weight: 600;
  border:none;
  appearance: none;
  width: 100%;
  outline: none;
  border-radius:0px 16px 0px 16px;
  background-color:#ddd3d3;
  transition:0.4s;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25)
}
.mysearch:focus{
  background-color:rgba(255, 255, 255, 0.479);
  box-shadow: 0px 0px 10px rgba(0,0,0,0.75);
  border-radius: 16px 0px 16px 0px;
}
.icon{
  color: red;
}
.startstyle{
  color: white;
  font-weight: 600;
}
.startstyle2{
  font-size: 10px;
  font-weight: 300;
}
.weather-details{
  text-align: center;
}
.location{
  color: #fff;
  font-size: 30px;
  font-weight:500;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
  text-align: center;
}
.date{
  color: #fff;
  font-weight: 300;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
  font-size:15px;
  padding-bottom:40px;
}
.temperature{
  text-align: center;
  font-size:130px;
  color:#fff;
  border-radius:25px;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  display: inline;
  font-weight: 200;
}
.mtemp{
  color: #fff;
}
.mtemp p{
  font-weight: 100;
}
.mtemp2{
  color: #fff;
}
.mtemp2 p{
  font-size:50px;
  margin-bottom: 0px!important;
}
.weather-box{
   text-align: center;  
}
.weather{
   font-weight:400;
   font-size:16px;
   color: #fff;
   text-shadow: 3px 6px rgba(0,0,0,0.25)
}
#app {
  background-image: url('./assets/map4.png');
  background-size: cover;
  background-position: bottom;
  transition:0.4s;
}
</style>
