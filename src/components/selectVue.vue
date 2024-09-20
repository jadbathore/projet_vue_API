<script>
import { ref } from 'vue'

export default {

  emits: ['emitChangeMethod'],
  methods:{
    /* 
    crée l'emit permettant de changer valeur 
    */
    emitChangeMethod() {
    this.$emit('emitChangeMethod',event.target.value);
  },
  }
}
</script>

<template >
  <select  class="bg-gray-50 m-2 border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
    <option @click="emitChangeMethod" value="all">Tous les lancements</option>
    <option @click="emitChangeMethod" value="success">Lancements réussis</option>
    <option @click="emitChangeMethod" value="failed">Lancements échoués</option>
  </select>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
