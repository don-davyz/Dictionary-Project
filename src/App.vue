<template>

  <div class="text-center mb-4">
    <img class="mb-4" src="logo.png" alt="" width="72" height="72">
    <h1 class="h3 mb-3 font-weight-normal">My Dictionary</h1>
    <p>Definitions from Free Dictionary API · <a target="_blank" href="https://dictionaryapi.dev/">Learn more</a></p>
  </div>
  <form @submit.prevent="getDefinations()" class="form-signin">
    <div class="form-label-group">
      <input style="border-radius: 0.3rem 0 0 0.3rem ;" type='text' v-model="word" class="st form-control" required autofocus>
      <button style="border-radius: 0 0.3rem 0.3rem 0;" class="ts btn btn-lg btn-primary btn-block"
        type="submit">Search</button>
    </div>
    <p style="color:red;" v-if="errortitle">{{ errortitle }}: <span>{{ errormessage }}</span></p>
  </form>
  <div class="text-justify mt-4">
    <div style="display:flex;">
      <img class="m-2" src="sound.svg" alt="sound" width="30px" height="auto">
      <h1 class="word font-weight-normal">{{ word2 }}</h1>
      <div class="m-3">{{ phonetic }}</div>
    </div>

    <h6 class="mt-5">Definitions</h6>
    <ol type="1">
      <li v-for="definition in definitionsArray" :key="definition">
        {{ definition }}
      </li>
    </ol>

    <h6 v-if="suggestionsArray">Suggestion:</h6>

    <span class="m-1 badge badge-default badge-outlined" v-for="suggestion in suggestionsArray" :key="suggestion">
      {{ suggestion }}
    </span>

  </div>


  <p class="mt-5 mb-3 text-muted text-center">Dictionary by Davies &copy; {{new Date().getFullYear()}}</p>



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
        this.word2 = response.data[0].word,
          this.phonetic = response.data[0].pronunciation,
          this.definitionsArray = [response.data[0].definition]
        console.log(response)
      })
      .catch(error => console.log(error))
      .finally(() => this.loading = false)
  },
  setup() {
    let errormessage, errortitle, phonetic, word2 = ref("");
    const suggestionsArray = ref([]);
    const definitionsArray = ref([]);
    const word = ref("");
    async function getDefinations() {

      // Empty Previous content
      definitionsArray.value.splice(0, definitionsArray.value.length);
      suggestionsArray.value.splice(0, suggestionsArray.value.length);
      this.errortitle = "";
      this.errormessage = '';

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
        this.word2 = responseOne.data[0].word;

      })).catch(errors => {
        // console.log(errors)
        this.errortitle = errors.response.data.title,
          this.errormessage = errors.response.data.message
      })


    }

    return { phonetic, errortitle, errormessage, suggestionsArray, definitionsArray, word, getDefinations, word2 }

  },


};

</script>

<style>
</style>
