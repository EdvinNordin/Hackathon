<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const info = ref(null)
const insult = ref(null)

onMounted(async () => {
  const response = await axios.get(
    'https://api.kanye.rest'
  )
  info.value = response.data.quote

  const evilinsult = await axios.get('https://api.adviceslip.com/advice')
  insult.value = evilinsult.data.slip.advice

})
</script>


<template>
 <div>
  <div>
  <p v-if=info>{{ info }}</p>
  </div>
  <div>
  <p v-if=insult>{{ insult }}</p>
  </div>
</div>


</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
