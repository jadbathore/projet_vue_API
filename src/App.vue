<script>

import selectVue from './components/selectVue.vue';
import nextLauch from './components/nextLauch.vue';
import modelLauch from './components/modelLauch.vue';

/* 
commencer l'application avec : npm start
*/

export default {
components: {
  nextLauch,
  selectVue,
  modelLauch

 },
data() {
  return {
        keyPointing:"",
        mountedData:{},
        modelData:{}  
  }
},
 methods:{
    /* 
    fetchdatamounted permet de fetch dés le chargement de la page les données 
    on fetch les donner puis par la suite on format un objet mountedData pour ne pas fetch a chaque fois au chargement je verifier si l'objet 
    mountedData n'est pas vide 
    */
    fetchDataMounted(){
      if(Object.keys(this.mountedData).length === 0)
      {
        fetch(`https://api.spacexdata.com/v4/launches/next`,{
          method:"GET"
        }).then((reponse) =>{
          reponse.json().then((data)=>{
            this.mountedData.image = data.links.patch.small
            this.mountedData.name = data.name
            this.mountedData.details = data.details
            this.mountedData.date = this.dateFormatter(data.date_utc)
            this.mountedData.flightNumber = data.flight_number
            this.mountedData.youtube_id = data.links.youtube_id;
            this.mountedData.payloads = data.payloads[0]
          });
        }).catch((err)=>{
          console.error(err)
        })
      }
    },
    /* 
    formate les date UTC en format DD/MM/AAAA 
    */
    dateFormatter(dateUTC){
      const date = new Date(dateUTC);
      const dateformat = `${date.getDay()}/${date.getMonth()}/${date.getFullYear()}`;
      return dateformat;
    },
    /* 
    fetch les donner selon la valeur prés selectionner par l'emit 'emitchangemethod' puis de la meme maniere que fetchDataMounted
    formatte un object si cette valeur n'est pas déja crée sinon elle change juste le poiteur keyPointing vers la valeur correspondante
    ()
    */
    fetchDataToSelect(valueSelected){
      if(typeof this.modelData[valueSelected] == 'undefined')
      {
        fetch(`https://api.spacexdata.com/v4/launches/past`,{
          method:"GET",
          'Content-Type': 'application/json'
        }).then((reponse) =>{
          reponse.json().then((data)=> {
            switch(valueSelected) {
              case 'all':data = data.slice(0,10);
              break;
              case 'success':data = data.filter((dataObj) => dataObj.success = true).slice(data.length-10,data.length);
              break;
              case 'failed': data = data.filter((dataObj) => dataObj.success = false).slice(data.length-10,data.length);

              break;
              default:throw new Error("select value undefined");
              break;
            }
            this.modelData[valueSelected] = [];
            for(let i = 0;i<data.length;i++)
            {
              const model = {};
              model.name = data[i].name;
              model.image = data[i].links.patch.small;
              model.details = data[i].details;
              model.date = this.dateFormatter(data[i].date_utc);
              model.article = data[i].links.article
              model.youtube_id = data[i].links.youtube_id;
              model.payload = data[i].payloads[0];
              this.modelData[valueSelected].push(model);
            }
            this.keyPointing = valueSelected;
          });
        }).catch((err)=>{
          console.error(err)
        })
      } else {
        this.keyPointing = valueSelected;
      }
    }
  },
  /* 
  permet deffectuer le chargement "onload" de la method 
  */
  mounted() {
    this.fetchDataMounted()
  }
}
</script>

<template >
<div class='p-5'>
  <div class='bg-gray-800 flex flex-col justify-center w-6/12 m-auto text-center rounded-lg mb-10 mb-10'>
    <!-- composant correspondant au prochain lancement préformater  -->
    <nextLauch :nextLauch="mountedData"/>
    <div class="w-9/12 m-auto p-10">
    <!-- composant correspondant au prochain a la selection avec un emit permettant le changement des composants  -->
    <selectVue @emitChangeMethod='fetchDataToSelect'/>
    </div>
  </div>
  <KeepAlive include="keyPointing">
    <div class="grid grid-cols-3 gap-3">
    <!--composant model looper représenant chaque élement du du modeldata (pour rappel un liste d'object composer en 3 clé (all,success,fail))  -->
      <modelLauch  v-if='modelData[keyPointing]' v-for='model in modelData[keyPointing]' :lauch='model'>  
      {{index}}
      </modelLauch>      
    </div>
  </KeepAlive>
</div>
</template>

<style scoped>

</style>
