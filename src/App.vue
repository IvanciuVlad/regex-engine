<template id="entire-page">
  <div id="app">
    <the-input></the-input>
    <the-engine></the-engine>
  </div>
</template>

<script>
import TheEngine from "@/components/TheEngine";
import TheInput from "@/components/TheInput";
import {ref, provide} from 'vue';

export default {
  components: {
    TheInput,
    TheEngine
  },
  setup() {
    const texts = ref({
      paragraph: 'a',
      regex: '.'
    });

    function addText(textData) {
      texts.value = {
        paragraph: textData.paragraph,
        regex: textData.regex
      }
    }

    provide('texts', texts);
    provide('addText', addText);
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
