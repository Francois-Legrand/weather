<template>
<div class="meteo" :class="{'background-lcouvert' : infos.weather[0].description === 'few clouds', 'background-couvert' : infos.weather[0].description === 'overcast clouds', 'background-broken' : infos.weather[0].description === 'broken clouds', 'background-scatt' : infos.weather[0].description === 'scattered clouds', 'background-clouds' : infos.weather[0].description === 'Clouds', 'background-rain' : infos.weather[0].description === 'Rain', 'background-mrain' : infos.weather[0].description === 'moderate rain', 'background-lrain' : infos.weather[0].description === 'light rain', 'background-csky' : infos.weather[0].description === 'clear sky'}">
    <div class="container">
        <div class="jour">
            <h1 class="text-white">Aujourd'hui</h1>
            <h2 class="text-white">à {{infos.name.slice(18,23)}}</h2>
        </div>
        <div>
            <img v-bind:src="imagem" alt="">
            <h2 class="text-white no-margin-top">{{infos.main.temp.toFixed(0)}}°</h2>
                <!--Clouds-->
            <h2 class="text-white no-margin-top" v-if="infos.weather[0].description === 'broken clouds'">Fragments de nuages</h2>
            <h2 class="text-white no-margin-top" v-if="infos.weather[0].description === 'overcast clouds'">Couvert</h2>
            <h2 class="text-white no-margin-top" v-else-if="infos.weather[0].description ==='Clouds'">Nuageux</h2>
            <h2 class="text-white no-margin-top" v-else-if="infos.weather[0].description ==='few clouds'">Quelques nuages</h2>
            <h2 class="text-white no-margin-top" v-else-if="infos.weather[0].description ==='scattered clouds'">Partiellement couvert</h2>
                <!--Rain-->
            <h2 class="text-white no-margin-top" v-else-if="infos.weather[0].description ==='light rain'">Légère pluie</h2>
            <h2 class="text-white no-margin-top" v-else-if="infos.weather[0].description === 'Rain'">Pluie</h2>
            <h2 class="text-white no-margin-top" v-else-if="infos.weather[0].description === 'moderate rain'">Pluie modérée</h2>
                <!--Sky-->
            <h2 class="text-white no-margin-top" v-else-if="infos.weather[0].description === 'clear sky'">Ciel clair</h2>
        </div>
        <div>
            <h2 class="text-white">prévisions</h2>
            <div class="row overflow padding-left"> 
                <div v-for="infojour in infosjour.list" :key="infojour.id" class="margin-right margin-bot">
                    <div class="flex-column">
                        <img :src="'https://openweathermap.org/img/wn/'+infojour.weather[0].icon+'.png'"/>
                        <div class="text-center text-white font-xs">{{ infojour.main.temp.toFixed(0)}}°</div>
                        <!--Rain-->
                        <div v-if="infojour.weather[0].description === 'light rain'" class="text-center text-white font-xs margin-top">Légère pluie</div>
                        <div v-else-if="infojour.weather[0].description === 'Rain'" class="text-center text-white font-xs margin-top">Pluie</div>
                        <div v-else-if="infojour.weather[0].description === 'moderate rain'" class="text-center text-white font-xs margin-top">Pluie modérée</div>
                        <!--Clouds-->
                        <div v-else-if="infojour.weather[0].description === 'Clouds'" class="text-center text-white font-xs margin-top">Nuageux</div>
                        <div v-else-if="infojour.weather[0].description === 'broken clouds'" class="text-center text-white font-xs margin-top">Fragments de nuages</div>
                        <div v-else-if="infojour.weather[0].description === 'overcast clouds'" class="text-center text-white font-xs margin-top">Couvert</div>
                        <div v-else-if="infojour.weather[0].description === 'few clouds'" class="text-center text-white font-xs margin-top">Quelques nuages</div>
                        <div v-else-if="infojour.weather[0].description === 'scattered clouds'" class="text-center text-white font-xs margin-top">Partiellement couvert</div>
                        <!--Sky-->
                        <div v-else-if="infojour.weather[0].description === 'clear sky'" class="text-center text-white font-xs margin-top">Ciel clair</div>
                        <div class="text-center width text-white font-xs margin-top">{{ format(infojour.dt_txt) }}</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

</template>

<script>

import axios from 'axios'
import {formatRelative} from 'date-fns'
import { fr } from 'date-fns/locale'
export default {
   name:"Meteo",
   data: function()
   {
       return{
           ok: this.imagem,
           infos: null,
           infosjour: null,
           image: "https://openweathermap.org/img/wn/04d@2x.png"
       }
   },
   mounted(){
       console.log(this.ok)
   },
   methods:{
       format(date){
           return formatRelative(new Date(date), new Date(), { locale : fr })
       }
   },
   computed: {
       imagem: function(){
           return this.infos ? "https://openweathermap.org/img/wn/"+this.infos.weather[0].icon+"@2x.png" : "";
           
       },
   },
   
   created(){
        axios.get("https://api.openweathermap.org/data/2.5/weather?q=Lens&units=metric&APPID=62ca4d47cef5a36196f8d74f12c1ae13")
        .then(response => (this.infos = response.data))

        axios.get("https://api.openweathermap.org/data/2.5/forecast?q=Lens,3003093&units=metric&APPID=62ca4d47cef5a36196f8d74f12c1ae13")
        .then(response => (this.infosjour = response.data))
        
   }
}
</script>
<style lang="css">
    body{
        margin: 0;
    }
    h1,h2{
       text-align: center; 
    }
    img{
        display: block;
        margin: auto;
    }

    .container{
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        height: 100vh;
       
    }
    .background-one{
        background-image: linear-gradient(to right bottom, #00b2ff, #0e8bd4, #1065aa, #0a4380, #002357);
    }
    .background-two{
        background-image: linear-gradient(to right bottom, #c9c9c9, #aeaacf, #8e8dd5, #6571db, #0058e0);
    }
    .background-lcouvert{
        height: 100vh;
        background-image: url(../assets/background-couvert.jpg);
    }
    .background-couvert{
        height: 100vh;
        background-image: url(../assets/couvert.jpg);
    }
    .background-broken{
        height: 100vh;
        background-image: url(../assets/broken-clouds.jpg);
    }
    .background-scatt{
        height: 100vh;
        background-image: url(../assets/scatt-cloud.jpg);
    }
    .background-clouds{
        height: 100vh;
        background-image: url(../assets/clouds.jpg);
    }
    .background-rain{
        height: 100vh;
        background-image: url(../assets/rain.jpg);
    }
    .background-mrain{
        height: 100vh;
        background-image: url(../assets/moderaterain.jpg);
    }
    .background-lrain{
        height: 100vh;
        background-image: url(../assets/light-rain.jpg);
    }
    .background-csky{
        height: 100vh;
        background-image: url(../assets/clear-sky.jpg);
    }
    .row{
        display: flex;
    }
    .flex-column{
        display: flex;
        flex-direction: column;
    }
    .text-center{
        text-align: center;
    }
    .margin-right{
        margin-right: 10px;
    }
    .width{
        width: 145px;
    }
    .overflow{
        overflow: scroll;
    }
    .text-white{
        color: rgb(255, 255, 255);
    }
    .font-xs{
        font-size: 0.8rem;
    }
    .margin-top{
        margin-top: 5px;
    }
    .margin-bot{
        margin-bottom: 0;
    }
    .no-margin-top{
        margin-top: 0;
    }
    .padding-left{
        padding-left: 10px;
    }
</style>