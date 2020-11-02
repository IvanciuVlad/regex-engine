<template>
  <section>
    <h3>Insert your text to be checked for match</h3>
    <form @submit.prevent="submitForm">
      <div>
        <label for="text">Text to be checked</label>
        <textarea rows="1" id="text" v-model="enteredText"/>
      </div>
      <div>
        <label for="regex">Regex expression</label>
        <textarea rows="1" id="regex" v-model="regexText" />
      </div>
      <button>Check</button>
    </form>
  </section>
</template>

<script >
import { ref, inject } from 'vue';

export default {
  setup() {
    const addText = inject('addText');

    const enteredText = ref('');
    const regexText = ref('');

    function submitForm() {
      addText({
        paragraph: enteredText,
        regex: regexText
      });
    }

    return {
      enteredText,
      regexText,
      submitForm
    }
  }
}
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

h3 {
  margin: 0.5rem 0;
}

input,
label,
textarea {
  display: block;
  width: 100%;
}

label {
  font-weight: bold;
}

input,
textarea {
  font: inherit;
  margin-bottom: 0.5rem;
}

button {
  font: inherit;
  background-color: #570080;
  border: 1px solid #570080;
  color: white;
  padding: 0.5rem 1.5rem;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #220031;
  border-color: #220031;
}
</style>
