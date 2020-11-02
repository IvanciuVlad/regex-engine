<template>
  <section>
    <h3>Does your regex expression validate the text:</h3>
    <button disabled class="false-res" v-if="!result">FALSE</button>
    <button disabled class="true-res" v-else>TRUE</button>
  </section>
</template>

<script>
import { inject, watch, ref } from 'vue';

export default {
  setup() {
    const loadedText = inject('texts');
    console.log(loadedText);

    let result = ref(null);

    watch(loadedText, function() {
      console.log("watch triggered");
      console.log(loadedText);
      console.log(loadedText.value.paragraph);
      console.log(loadedText.value.regex);
      result.value = search(loadedText.value.regex, loadedText.value.paragraph);
      console.log(result);
    });

    function search(pattern, text) {
      if (pattern[0] === '^') {
        return match(pattern.slice(1), text);
      } else {
        return match('.*' + pattern, text);
      }
    }

    function matchOne(pattern, text) {
      if (!pattern) return true;
      if (!text) return false;
      return pattern === '.' || text === pattern;
    }

    function match(pattern, text) {
      if (!pattern) return true;
      else if (!text && pattern === '$') return true;
      else if (pattern[1] === '?') {
        return matchQuestion(pattern, text);
      } else if (pattern[1] === '*') {
        return matchStar(pattern, text);
      } else if (pattern[0] === '(') {
        return matchGroup(pattern, text);
      } else {
        return matchOne(pattern[0], text[0]) && match(pattern.slice(1), text.slice(1));
      }
    }

    function matchQuestion(pattern, text) {
      return (
        (matchOne(pattern[0], text[0]) && match(pattern.slice(2), text.slice(1))) ||
        match(pattern.slice(2), text)
      );
    }

    function matchStar(pattern, text) {
      return (
        (matchOne(pattern[0], text[0]) && match(pattern, text.slice(1))) ||
        match(pattern.slice(2), text)
      );
    }

    function matchGroup(pattern, text) {
      const groupEnd = pattern.indexOf(')');
      const groupPattern = pattern.slice(1, groupEnd);
      if (pattern[groupEnd + 1] === '?') {
        const remainderPattern = pattern.slice(groupEnd + 2); // +2 needed to slice off the ')?'
        return (
          (match(groupPattern, text.slice(0, groupPattern.length)) &&
            match(remainderPattern, text.slice(groupPattern.length))) ||
          match(remainderPattern, text)
        );
      } else if (pattern[groupEnd + 1] === '*') {
        const remainderPattern = pattern.slice(groupEnd + 2); // +2 needed to slice off the ')*'
        return (
          (match(groupPattern, text.slice(0, groupPattern.length)) &&
            match(pattern, text.slice(groupPattern.length))) ||
          match(remainderPattern, text)
        );
      } else {
        const remainderPattern = pattern.slice(groupEnd + 1); // +1 needed to slice off the ')'
        return (
          match(groupPattern, text.slice(0, groupPattern.length)) &&
          match(remainderPattern, text.slice(groupPattern.length))
        );
      }
    }

    return { text: loadedText, result };
  }
};
</script>

<style scoped>
section {
  margin: 3rem auto;
  max-width: 40rem;
  padding: 1rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  background-color: azure;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  margin: 1rem 0;
  border: 1px solid #ccc;
  padding: 1rem;
}

h3 {
  margin: 0.5rem 0;
}

button {
  padding: 15px 32px;
  margin: 1rem 0;
  border: 1px solid #ccc;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  color: white;
  font-size: 24px;
  border-radius: 8px;
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
}

.false-res {
  background-color: red;
}

.true-res {
  background-color: green;
}
</style>
