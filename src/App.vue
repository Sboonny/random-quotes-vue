
<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios"
import { ref } from "vue"
export default defineComponent({
  setup() {

    const quotes: any = ref('')
    const author: any = ref('')
    const data: any = ref("")
    let number: number = 0

    const loadQuote = async () => {
      const response = await axios.get("https://gist.githubusercontent.com/camperbot/5a022b72e96c4c9585c32bf6a75f62d9/raw/e3c6895ce42069f0ee7e991229064f167fe8ccdc/quotes.json")

      let prevNumb = number
      number = Math.ceil(Math.random() * 65)

      if (prevNumb === number) loadQuote();

      quotes.value = response.data.quotes[number].quote
      author.value = response.data.quotes[number].author
      data.value = response
    }
   
    loadQuote()




    return {
      quotes,
      author,
      data,
      number,
      loadQuote
    }
  }
})
</script>


<template>
  <main id="app">
    <figure id="quote-box">
      <blockquote id="text" :key="number">{{ quotes }}</blockquote>
      <figcaption id="author" :key="number">{{ author }}</figcaption>
      <button id="new-quote" @click="loadQuote" :key="number">click</button>
      <a href="twitter.com/intent/tweet" id="tweet-quote"></a>
    </figure>
  </main>
</template>

<style>
*,
*::after,
*::before {
  box-sizing: border-box;
}
main {
  display: grid;
  place-content: center;
  height: 100vh;
}
figure {
  width: 40vw;
}
</style>