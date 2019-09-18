<template>
   <div class="bitcoin">
       <h2>Comparaison Bitcoin</h2>
    Euros : <span v-if="infos">{{infos.bpi.EUR.rate}} Euros</span>
       <!--<button  @click="getInfos">Récuperer</button>-->
   </div>
</template>
<script>

import axios from 'axios'

export default {
   name:"Bitcoin",
   data: function()
   {
       return{
           infos: null
       }
   },
   methods:{
       getInfos:function()
       {
           axios.get("https://api.coindesk.com/v1/bpi/currentprice.json")
           .then(response => (this.infos = response.data))
           console.log(this.infos)
       }
   },
   created(){
        axios.get("https://api.coindesk.com/v1/bpi/currentprice.json")
           .then(response => (this.infos = response.data))
   }
}
</script>
