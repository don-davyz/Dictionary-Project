<template>
<form @submit.prevent="getDefinitions()" >
<input placeholder="Enter Word" type='text' id='word' v-model="word"/>
<button type='submit'>Meaning</button>
</form>
</template>

<script>
import { ref } from "vue";

export default {
  name: 'App',
  setup(){
    const word = ref("");

    return{ word };
  }
 
};
</script>

<style>

</style>
