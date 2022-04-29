<template>
  <form @submit.prevent="method1()" class="form-signin">
    <div class="text-center mb-4">
      <img class="mb-4" src="https://getbootstrap.com/docs/4.0/assets/brand/bootstrap-solid.svg" alt="" width="72"
        height="72">
      <h1 class="h3 mb-3 font-weight-normal">My Dictionary</h1>
      <p>Build form controls with floating labels via the</p>
    </div>

    <div class="form-label-group">
      <input type='text' id='word' v-model="word" class="form-control" placeholder="Enter word" required autofocus>
      <button class="btn btn-lg btn-primary btn-block" type="submit">Search</button>
    </div>
    <div class="text-center mb-4">
      <h1 class="h3 mb-3 font-weight-normal">Fishes</h1>
      <p>A fish is a fish <code>:placeholder-shown</code> pseudo-element. <a
          href="https://caniuse.com/#feat=css-placeholder-shown">Works in latest Chrome, Safari, and Firefox.</a></p>
    </div>

    
    <p class="mt-5 mb-3 text-muted text-center">Dictionary by Davies &copy; 2022</p>
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
