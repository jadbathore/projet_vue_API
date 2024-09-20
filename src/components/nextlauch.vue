<script>
import { ref } from 'vue'

export default {
    props:{
      nextLauch:{
      }
    },
    methods:{
      createUrlytApi(yt_origin) {
        return `http://www.youtube.com/embed/${yt_origin}?enablejsapi=1`;
      },
    }
}
</script>

<template>
    <img class="m-auto my-4 w-2/12" v-if="nextLauch.image" :src='nextLauch.image'>
    <h2 class="m-2 text-4xl font-bold tracking-tight text-gray-900 dark:text-white">Prochain lancement</h2>
    <p class="m-2 font-normal text-gray-700 dark:text-gray-400" v-if="nextLauch.name">{{nextLauch.name}}</p>
    <p class="m-2 font-normal text-gray-700 dark:text-gray-400"v-if="nextLauch.date">{{nextLauch.date}}</p>
    <p class="m-2 font-normal text-gray-700 dark:text-gray-400"  v-if="nextLauch.details">{{nextLauch.details}}</p>
    <p class="m-2 font-normal text-gray-700 dark:text-gray-400"  v-if="nextLauch.flightNumber">fightNumber:{{nextLauch.flightNumber}}</p>
    <p class="m-2 font-normal text-gray-700 dark:text-gray-400"  v-if="nextLauch.payloads">payloads:{{nextLauch.payloads}}</p>
    <iframe class="h-[300px]"  :src="createUrlytApi(nextLauch.youtube_id)">youtube link</iframe>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
