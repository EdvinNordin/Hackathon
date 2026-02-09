<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'


const displayedQuote = ref('')
const correctAnswer = ref('')
const result = ref('')

async function getDesignQuote() {
const response = await axios.get('https://quotesondesign.com/wp-json/wp/v2/posts/')

  const raw = response.data[0].content.rendered
  return raw.replace(/<[^>]+>/g, '').trim()
}

onMounted(async () => {
  const kanyeResponse = await axios.get('https://api.kanye.rest')
  const adviceResponse = await axios.get('https://api.adviceslip.com/advice')
  const designQuote = await getDesignQuote()

  const kanyeQuote = kanyeResponse.data.quote
  const adviceQuote = adviceResponse.data.slip.advice

  const quotes = [
    { text: kanyeQuote, source: 'kanye' },
    { text: adviceQuote, source: 'not-kanye' },
    { text: designQuote, source: 'not-kanye' }
  ]

 
  const randomIndex = Math.floor(Math.random() * quotes.length)
  displayedQuote.value = quotes[randomIndex].text
  correctAnswer.value = quotes[randomIndex].source
})

function guess(answer) {
  result.value = answer === correctAnswer.value ? 'Rätt! 🎉' : 'Fel 😬'
}
</script>

<template>
  <div>
    <h1>Guess-Ye😃</h1>
  <div>

    <h2>Vem sa det här?</h2>

    <p v-if="displayedQuote" class="quote">“{{ displayedQuote }}”</p>

    <button @click="guess('kanye')">Kanye West</button>
    <button @click="guess('not-kanye')">Någon annan</button>

    <p v-if="result" :class="['result', result === 'Rätt! 🎉' ? 'correct' : 'wrong']">
      {{ result }}
    </p>
  </div>
</div>
</template>

<style scoped>
* {
  box-sizing: border-box;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  margin: 0;
  padding: 0;
}

body {
  background: #f5f5f5;
  color: #333;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 2rem;
}

h1 {
  text-align: center;
  color: #87CEEB;
  margin-bottom: 1.5rem;
  font-size: 4rem;
  font-family: sans-serif;
}

h2 {
  text-align: center;
  margin-bottom: 1.5rem;
  font-size: 2rem;
  color: #1a1a1a;
}

.quote {
  background: #fff;
  padding: 1rem 1.5rem;
  margin-bottom: 1.5rem;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  font-size: 1.25rem;
  text-align: center;
  font-style: italic;
  color: #2c3e50; 
  transition: transform 0.2s ease, box-shadow 0.2s ease, color 0.2s ease;
}

.quote:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.15);
  color: #3498db; 
}

button {
  padding: 0.75rem 2rem;
  margin: 0.5rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.2s ease;
  background: linear-gradient(135deg, #ff8a00, #e52e71);
  color: white;
  font-weight: bold;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

button:active {
  transform: translateY(0);
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.result {
  font-size: 1.5rem;
  font-weight: bold;
  text-align: center;
  margin-top: 1rem;
  animation: fadeIn 0.5s ease-in-out;
}

.result.correct {
  color: #27ae60; 
}

.result.wrong {
  color: #e74c3c; 
}

@keyframes fadeIn {
  0% { opacity: 0; transform: translateY(-10px); }
  100% { opacity: 1; transform: translateY(0); }
}

@media (min-width: 768px) {
  .quote {
    font-size: 1.5rem;
    max-width: 600px;
  }

  button {
    font-size: 1.1rem;
    padding: 0.85rem 2.5rem;
  }
}
</style>
