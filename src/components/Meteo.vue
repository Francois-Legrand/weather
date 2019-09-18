<template>
   <div class="meteo">
    <div class="container">
        <h1>Aujourd'hui</h1>
        <img v-bind:src="imagem" alt="">
        <h2 v-if="infos">à {{infos.name}}</h2>
        <h2 v-if="infos">{{infos.weather[0].description}}</h2>
        <h2 v-if="infos">{{infos.main.temp}}C°</h2>
        <h2 v-for="inf in infosjour" :key="inf.id"></h2>
        <!--<button  @click="getInfos">Récuperer</button>-->
    </div>
   </div>

</template>

<script>

import axios from 'axios'

export default {
   name:"Meteo",
   data: function()
   {
       return{
           infos: null,
           infosjour: null,
           image: "https://openweathermap.org/img/wn/04d@2x.png"
       }
   },
   computed: {
       imagem: function(){

           return this.infos ? "https://openweathermap.org/img/wn/"+this.infos.weather[0].icon+"@2x.png" : ""
       }
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
    h1,h2{
       text-align: center; 
    }
    img{
        display: block;
        margin: auto;
    }

    .container{
        padding:20px;
        width: 500px;
        background-color: rgb(231, 230, 230);
    }
</style>