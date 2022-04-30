<template>

  <div class="text-center mb-4">
    <img class="mb-4" src="logo.png" alt="" width="72" height="72">
    <h1 class="h3 mb-3 font-weight-normal">My Dictionary</h1>
    <p>Definitions from Free Dictionary API · <a target="_blank" href="https://dictionaryapi.dev/">Learn more</a></p>
  </div>
  <form @submit.prevent="getDefinations()" class="form-signin">
    <div class="form-label-group">
      <input style="border-radius: 0.3rem 0 0 0.3rem ;" type='text' v-model="word" class="st form-control"
        placeholder="Enter word" required autofocus>
      <button style="border-radius: 0 0.3rem 0.3rem 0;" class="ts btn btn-lg btn-primary btn-block"
        type="submit">Search</button>
    </div>
    <p style="color:red;" v-if="errortitle">{{ errortitle }}: <span>{{ errormessage }}</span></p>
  </form>
  <div class="text-justify mt-4">
    <div style="display:flex;">
      <img class="m-2" src="sound.svg" alt="sound" width="30px" height="auto">
      <h1 class="word font-weight-normal">{{ word }}</h1>
      <div class="m-3">{{ phonetic }}</div>
    </div>

    <h6 class="mt-5">See definitions:</h6>

    <div v-for="definition in definitionsArray" :key="definition">
      <p>{{ definition }}</p>
    </div>

    <h6>Suggestion:</h6>

    <span class="badge badge-default badge-outlined" v-for="suggestion in suggestionsArray" :key="suggestion">
      {{ suggestion }}
    </span>

  </div>


  <p class="mt-5 mb-3 text-muted text-center">Dictionary by Davies &copy; 2022</p>



</template>

<script>
import axios from 'axios';
import { ref } from "vue";

export default {
  name: 'App',

  mounted() {
    this.loading = true;
    axios
      .get('https://random-words-api.vercel.app/word')
      .then(response => {
        this.word = response.data[0].word,
          this.phonetic = response.data[0].pronunciation,
          this.definitionsArray = [response.data[0].definition]
        console.log(response)
      })
      .catch(error => console.log(error))
      .finally(() => this.loading = false)
  },
  setup() {
    let errormessage, errortitle, phonetic = ref("");
    const suggestionsArray = ref([]);
    const definitionsArray = ref([]);
    const word = ref("");
    async function getDefinations() {

      const requestOne = axios.get('https://api.dictionaryapi.dev/api/v2/entries/en/' + word.value);
      const requestTwo = axios.get('https://api.datamuse.com/sug?s=' + word.value);
      await axios.all([requestOne, requestTwo]).then(axios.spread((...responses) => {
        const responseOne = responses[0]
        const responseTwo = responses[1]

        responseOne.data[0].meanings.forEach(({ definitions }) => {
          definitions.forEach(({ definition }) => {
            if (typeof definition === "string")
              definitionsArray.value.push(definition);
          });
        });

        responseTwo.data.forEach(({ word }) => {
          if (typeof word === "string")
            suggestionsArray.value.push(word);

        });

        this.phonetic = responseOne.data[0].phonetic;

      })).catch(errors => {
        console.log(errors)
        this.errortitle = errors.response.data.title,
          this.errormessage = errors.response.data.message
      })


    }

    return { phonetic, errortitle, errormessage, suggestionsArray, definitionsArray, word, getDefinations }

  },


};

</script>

<style>
</style>
