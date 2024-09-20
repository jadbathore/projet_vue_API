<script>
import { ref } from 'vue'

export default {
    data() {
      return {
          show:false
      }
    },
    props:{
      lauch:{},
    },
    methods:{
      //formate l'url de iframe permetant de visualiser les vidéo yt
      createUrlytApi(yt_origin) {
      return `http://www.youtube.com/embed/${yt_origin}?enablejsapi=1`;
      },
      //crée un toggle chaque fois actionner le boolean inverse se produit  
      showHide(){
        this.show = !this.show;
      }
    }
}

</script>

<template>
<!-- format d'un vignette contenant une mission -->
<div class="flex flex-col items-center bg-white border border-gray-200 rounded-lg shadow md:flex-row md:max-w-xl hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700">
    <img class="object-cover w-full rounded-t-lg h-96 md:h-auto md:w-48 md:rounded-none md:rounded-s-lg" v-if="lauch.image" :src='lauch.image'>
    <div class="flex flex-col justify-between p-3 leading-normal">
    <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white" v-if="lauch.name">{{lauch.name}}</h5>
    <p class="mb-3 font-normal text-gray-700 dark:text-gray-400" v-if="lauch.date">{{lauch.date}}</p>
    <p class="mb-3 font-normal text-gray-700 dark:text-gray-400" v-if="lauch.details">{{lauch.details}}</p>
    <p class="mb-3 font-normal text-gray-700 dark:text-gray-400" v-if="lauch.payloads">{{lauch.payloads}}</p>
    <div class="flex flex-col w-[300px]">
    <a class="m-1 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full text-center" :href="lauch.article">article</a>
    <!-- bouton permettant de montré  ou ferme l'iframe -->
    <button @click='showHide()'class="m-1 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full" type="button">
    <p v-if="!show">vidéo youtube</p>
    <p v-if="show">X</p>
    </button>
    <div v-if="show" class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600">
      <iframe  :src="createUrlytApi(lauch.youtube_id)">youtube link</iframe>
    </div>
    </div>
    </div>
</div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>