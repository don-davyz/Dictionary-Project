<template>
  <form @submit.prevent="method1()">
    <input placeholder="Enter Word" type='text' id='word' v-model="word" />
    <button type='submit'>Meaning</button>
  </form>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  mounted() {
    this.loading = true;
    axios
      .get('https://random-words-api.vercel.app/word')
      .then(response => console.log(response))
      .catch(error => console.log(error))
      .finally(() => this.loading = false)
  },
  methods: {
    method1: function () {
      
      const word = document.getElementById('word').value;
      axios
        .get('https://api.dictionaryapi.dev/api/v2/entries/en/' + word.value)
        .then(response2 => console.log(response2))
        .catch(error2 => console.log(error2))

      axios
        .get('https://api.datamuse.com/sug?s=' + word.value)
        .then(response3 => console.log(response3))
        .catch(error => console.log(error))
    }
  }

};

</script>

<style>
</style>
