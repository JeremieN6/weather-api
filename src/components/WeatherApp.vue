<template>
        <div class="weather_app">
            <!-- SIDE MENU -->
            <div id="mySidenav" class="sidenav mySidenav_desktop mySidenav_mobile">
                <a href="javascript:void(0)" class="closebtn" v-on:click="closeNav">&times;</a>
                <div class="searchmenu">
                    <div class="input-icons">
                        <i class="bi bi-search icon"></i>
                        <input v-model="currentCity" class="input-field" type="text" placeholder="Entrez un lieu">
                    </div>
                        
                    <div class="search_button">
                        <button class="btn btn-primary" v-on:click="submitCityName">Chercher</button>
                    </div>
                </div>
            </div>
            <!-- SIDE MENU -->

            <!-- HEADER -->
            <div class="header">
                <div class="nav">
                    <div v-on:click="openNav" class="btn btn-custom">Rechercher un lieu</div>
                    <div class="icon_nav"><i class="bi bi-compass"></i></div>
                </div>
                <div class="bg-img">
                    <img class="bg-img-default" src="../assets/images/Cloud-background.png" alt="Cloud-background">
                </div>
                <div class="data_weather">
                    <img class="current_img_weather" src="../assets/images/Clear.png" alt="Current Weather">
                
                <div class="degree" id="degreeC">
                    <h2>{{cityCurrentTemp.temp_c}} <span>°C</span></h2>
                </div>
                <div class="degree" id="degreeF">
                    <h2>{{cityCurrentTemp.temp_f}} <span>°F</span></h2>
                </div>
                <div class="weather_descr">
                    <p>{{cityCurrentText.text}}</p>
                </div>
                <div class="target_date">
                    <div class="date">Aujourd'hui</div>
                    <div class="date_space">•</div>
                    <div class="full_date">26 Janv 2023</div>
                </div>
                <div class="target_city">
                    <div class="icon_target_city">
                        <i class="bi bi-geo-alt"></i>
                    </div> 
                    <div class="targeted_city">{{currentCityName.name}}</div>
                </div>
                </div>
            </div>
            <!-- HEADER -->

            <!-- WEATHER PREDICTIONS AND HIGHLIGHTS MEASURE -->
            <div class="content">
                <div class="buttons_temp">
                    <div class="button_temperature_active" id="button_temperatureC" v-on:click="celsius">°C</div>
                    <div class="button_temperature_inactive" id="button_temperatureF" v-on:click="fahrenheit">°F</div>
                </div>
                <div class="weather_prediction"> <!--v-for="item in currentCity" :key="currentCity" -->
                    <!-- Section 1 -->
                        <div class="card_details">
                            <div class="detailed_date">Lundi</div>
                            <div class="card_detail_img">
                                <img class="detailed_img" src="../assets/images/LightCloud.png" alt="LightCloud">
                            </div>
                            <div class="temperature_day">
                                <div class="temperature_morning">30°C</div>
                                <div class="temperature_afternoon">27°C</div>
                            </div>
                        </div>
                    <!-- Section 1 -->
                </div>
                <!-- WEATHER PREDICTIONS END -->

                <!-- HIGHLIGHTS MEASURE -->
                <div class="container">
                    <div class="container title_hl">
                        <h3>Points forts du jour</h3>
                    </div>
                    <div class="highlights_container">
                        <div class="highlights_cards">
                            <div class="highlights_status">
                                <div class="hl_Wind">Etat du Vent</div>
                                <div class="hl_WindValue">92<span>Km/H</span> </div>
                                <div class="hl_WindDirection">{{cityCurrentTemp.wind_dir}}</div>
                            </div>
                        </div>
                        <div class="highlights_cards">
                            <div class="highlights_status">
                                <div class="hl_Humidity">Humidité</div>
                                <div class="hl_WindValue">{{cityCurrentTemp.humidity}}<span>%</span> </div>
                                <div class="d-flex flex-column w-50">
                                    <div class="progress">
                                        <div class="progress-bar progress-bar-striped progress-bar-animated" role="progressbar" aria-valuenow="69" aria-valuemin="0" aria-valuemax="100" style="width: 69%; background-color: #FFEC65;">
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="highlights_cards">
                            <div class="highlights_status">
                                <div class="hl_Wind">Visibility</div>
                                <div class="hl_WindValue">{{cityCurrentTemp.vis_km}}<span>Km</span> </div>
                            </div>
                        </div>
                        <div class="highlights_cards">
                            <div class="highlights_status">
                                <div class="hl_Wind">Pression de l'air</div>
                                <div class="hl_WindValue">{{cityCurrentTemp.pressure_mb}}<span>mb</span> </div>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- HIGHLIGHTS MEASURE -->
                <footer>
                    <p> <strong>JereMétéo</strong> - par <i>Jérémie NGOYI MAOLE</i> - Développeur Full Stack</p>
                </footer>
            </div>
            <!-- WEATHER PREDICTIONS AND HIGHLIGHTS MEASURE END -->
        </div>
</template>

<script>
  import axios from 'axios'
export default {
  name: 'WeatherApp',
  data(){
    return{
      cityCurrentTemp: {},
      city:{},
      cityCurrentText: {},
      currentCity: "",
      currentCityName: {},
      isCelsuis: true,
    }
  },
  mounted(){
    var cityInserted = "Ajaccio";
    var apiKey = "056e3720f3ba46a2961185008220507";
    axios
    .get(`https://api.weatherapi.com/v1/current.json?key=` + apiKey + `&q=` + cityInserted)
    .then((response) =>{
      this.currentCityName = response.data.location;
      this.cityCurrentTemp = response.data.current;
      this.cityCurrentText = response.data.current.condition;
    //   console.log(this.cityCurrentTemp)
    //   console.log(this.currentCityName)
    })
  },
  methods:{
    submitCityName(){
        var apiKey = "056e3720f3ba46a2961185008220507";
        var cityInserted = this.currentCity;
        var dayNumber = 3;
    axios
    .get(
      `https://api.weatherapi.com/v1/forecast.json?key=` +
           apiKey +
           `&q=` +
           cityInserted +
           `&days=` +
           dayNumber)
    .then((response) =>{
      this.currentCity = response.data.forecast.forecastday;
      this.city = response.data.location;
         console.log(this.city);
         console.log(this.currentCity);
    })
    },
     celsius(){
        var celsius = document.getElementById("button_temperatureC");
        var fahrenheit = document.getElementById("button_temperatureF");
        var dataFar = document.getElementById("degreeF");
        var dataCel = document.getElementById("degreeC");
        
        fahrenheit.classList.add("button_temperature_inactive");
        celsius.classList.add("button_temperature_active");

        fahrenheit.classList.remove("button_temperature_active");
        celsius.classList.remove("button_temperature_inactive");

        dataFar.style.display = "none";
        dataCel.style.display = "block";

    },
     fahrenheit(){
        var celsius = document.getElementById("button_temperatureC");
        var fahrenheit = document.getElementById("button_temperatureF");
        var dataFar = document.getElementById("degreeF");
        var dataCel = document.getElementById("degreeC");
    
        fahrenheit.classList.add("button_temperature_active");
        celsius.classList.add("button_temperature_inactive");

        fahrenheit.classList.remove("button_temperature_inactive");
        celsius.classList.remove("button_temperature_active");

        dataFar.style.display = "block";
        dataCel.style.display = "none";


    },
    openNav() {
        document.getElementById("mySidenav").style.visibility = "visible";
        document.getElementById("mySidenav").style.opacity = 1;
        if ((window).width() < 768) { //$(window)
            document.getElementById("mySidenav").classList.remove('mySidenav');
            document.getElementById("mySidenav").classList.add('mySidenav_mobile');
            }
            else {
                console.log("Erreur Side Menu")
            }
    },
    closeNav() {
        document.getElementById("mySidenav").style.visibility = "hidden";
        document.getElementById("mySidenav").style.opacity = 0;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.weather_app{
    display: flex;
    flex-direction: column;
}
.header{
    background-color: #1E213A;
    /* position: absolute; */
    width: auto;
    height: 510px;
    left: 0px;
    top: 0px;
}
.nav{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
.icon_nav{
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 1em 0em 0;
    border: 1px solid #88869D;
    background-color: #88869D;
    width: 2em;
    height: 2em;
    border-radius: 50%;
    color: white;
    cursor: pointer;
}
.searchmenu{
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}
.input-icons i {
    position: absolute;
}
 
.input-icons i {
    position: absolute;
}
input, select, textarea{
    color: #ffffff;
}
.input-icons {
    width: 50%;
    display: flex;
}
 
.icon {
    padding: 10px;
    color: grey;
    min-width: 50px;
    text-align: center;
}
 
.input-field {
    width: 90%;
    padding: 10px;
    text-align: center;
    background-color: #1E213A;
    border: 1px solid white;
}
/* .input_search_field{
    border: 1px solid #E7E7EB; 
    background-color: #1E213A;
    height: 2.2em;
    position: relative;
    display:inline-block;
    width: 100%;
    padding: 10px;
    text-align: center;
} */
h1,h2 .header{
    font-family: 'Raleway';
    font-style: normal;
    font-weight: 500;
    font-size: 80px;
    line-height: 80px;
    color: white;
}
h2{
    font-size: 5rem;
    color: white;
}
h4,p .header{
    color: #88869D;
}
span {
    color: #E7E7EB;
    font-size: 20px;

}
.nav{
    display: flex;
    flex-direction: row;
    margin-top: 1em;
    margin-left: 1em;
}
.btn-custom{
    background-color: #6E707A;
    border: #6E707A;
    color: white;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
}
.bg-img{
    display: flex;
    justify-content: center;
    z-index: 0;
    
}
.bg-img-default{
    width: 99%;
    opacity: 0.10;
}
.data_weather{
    display: flex;
    justify-content: center;
    flex-direction: column;
    /* flex-direction: row; */
    align-items: center;
    /* z-index: 1; */
    margin-top: -11em;
}
.current_img_weather{
   width: 40%;
}
.degree{
    display: flex;
}
#degreeF{
    display: none;
}
.weather_descr{
    color: #88869D;
    font-size: 26px;
    margin: 0 0 0 0.35em;
}
.target_date,.target_city{
    display: flex;
    flex-direction: row;
    justify-content: center;
    margin: 0 0 0 0.35em;
    color: #88869D;
}
.date_space,.icon_target_city{
    margin-right: .5em;
    margin-left: .5em;
    margin-bottom: 1em;
}
.content{
    background-color: #100E1D;
    height: max-content;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
}
.buttons_temp{
    display: flex;
    flex-direction: row;
    justify-content: flex-end;
    margin: 1em 4.5em 0 0;
}
.button_temperature_active{
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 1em 0em 0;
    border: 1px solid white;
    background-color: white;
    width: 2em;
    height: 2em;
    border-radius: 50%;
    color: #110E3C;
    font-size: 15px;
    font-weight: bold;
    cursor: pointer;
}
.button_temperature_inactive{
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 1em 0em 0;
    border: 1px solid #585676;
    background-color: #585676;
    width: 2em;
    height: 2em;
    border-radius: 50%;
    color: white;
    font-size: 15px;
    font-weight: bold;
    cursor: pointer;
}
.weather_prediction{
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    width: 100%;
}
.card_details{
    display: flex;
    flex-direction: column;
    margin: 2em .5em 2em .5em;
    /* height: 177px; */
    width: 120px;
    left: 54px;
    top: 862px;
    border-radius: 0px;
    background-color: #1E213A;
    color: white;
}
.detailed_date{
    display: flex;
    justify-content: center;
    margin-top: 1em;
}
.card_detail_img{
    display: flex;
    justify-content: center;
    margin-top: 1em;
    margin-bottom: 1em;
}
.detailed_img{
    width: 50%;
}
.temperature_day{
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    margin-bottom: 1em;
}
.temperature_afternoon{
    color: #A09FB1;
}
.title_hl{
    color: white;
    margin-bottom: 1em;
}
.highlights_container{
    display: flex;
    flex-direction: column;
    justify-content: center;
}
.highlights_cards{
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    color: white;
    margin-bottom: 2em;
    background-color: #1E213A;
}
.highlights_status{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 1em 0 1em 0;
    width: 100%;
}
.hl_WindValue{
    font-size: 50px;
    margin: .5em 0 .5em 0;
}
.progress{
    border-radius: 2.25rem;
}
footer{
    color: white;
    font-size: 10px;
    display: flex;
    justify-content: center;
}
.mySidenav_desktop{
    width: 35%;
    visibility: hidden;
    opacity: 0;
    transition: visibility 0s, opacity 0.5s ease-in-out;
}

.sidenav {
    height: 100%;
    width: 35%;
    position: fixed;
    z-index: 1;
    top: 0;
    left: 0;
    background-color: #1E213A;
    overflow-x: hidden;
    transition: 0.5s;
    padding-top: 60px;
}

.sidenav a {
    padding: 8px 8px 8px 32px;
    text-decoration: none;
    font-size: 25px;
    color: #818181;
    display: block;
    transition: 0.3s;
}

.sidenav a:hover {
    color: #f1f1f1;
}

.sidenav .closebtn {
    position: absolute;
    color: #ffffff;
    top: 0;
    right: 25px;
    font-size: 36px;
    margin-left: 50px;
}

@media screen and (max-height: 450px) {
.sidenav {padding-top: 15px;}
.sidenav a {font-size: 18px;}
}
@media (min-width:320px) and (max-width:767px){

    .mySidenav_mobile{
        width: 100%;
        visibility: hidden;
        opacity: 0;
        transition: visibility 0s, opacity 0.5s ease-in-out;
    }
    .input-field {
        font-size: 12px;
    }
}
@media screen and (min-width:768px) {
    .header{
        width: 45%;
        height: 100vh;
    }
    .weather_app{
        display: flex;
        flex-direction: row;
    }
    .content {
        background-color: #100E1D;
        width: 55%;
        height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
    }
    .highlights_cards{
        display: flex;
        flex-direction: column;
        margin: 1em 1em 1em 0;
        width: 30%;
    }
    .highlights_container{
        display: flex;
        flex-wrap: wrap;
        flex-direction: row;
    }
    .mySidenav_disable{
        width: 0%;
    }
    #degreeF{
    display: none;
}
}
@media screen and (min-width:1024px) {
    .header{
        width: 35%;
    }
    .content {
        background-color: #100E1D;
        width: 65%;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
    }
    .highlights_cards{
        display: flex;
        flex-direction: column;
        margin: 1em 1em 1em 0;
        width: 30%;
    }
    .highlights_container{
        display: flex;
        flex-wrap: wrap;
        flex-direction: row;
    }
    /* .sidenav{
        width: 100%;
    }
    #mySidenav{
        width: 100%;
    } */
}
@media (max-width:767px){
    .buttons_temp{
    display: none;
}
}
</style>
