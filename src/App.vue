<template>
<form @submit.prevent="getDefinitions()" >
<input placeholder="Enter Word" type='text' id='word'/>
<button type='submit'>Meaning</button>
</form>
</template>

<script>

export default {
  name: 'App',
 
};
</script>

<style>

</style>
