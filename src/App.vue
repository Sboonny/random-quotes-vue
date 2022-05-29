
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
  <main>
    <figure id="quote-box">
      <blockquote id="text" :key="number">{{ quotes }}</blockquote>
      <figcaption id="author" :key="number">- {{ author }}.</figcaption>
      <a href="twitter.com/intent/tweet" id="tweet-quote">
        <picture>
          <source media="(prefers-color-scheme: dark)" srcset="twitter-dark-scheme.png" alt="twitter">
        <img src="twitter-light-scheme.png" alt="twitter">
        </picture>
        </a>
      <button id="new-quote" @click="loadQuote" :key="number">NEW QUOTE</button>
    </figure>
  </main>
</template>

<style>
:root {
  --bg-color: hsl(195, 20%, 75%) ;
  --ft-color: hsl(308, 80%, 15%);
  --fig-color: hsl(29, 100%, 77%) ;
}

@media (prefers-color-scheme: dark) {
  :root {
  --bg-color: hsl(195, 50%, 6%) ;
  --ft-color: hsl(308, 15%, 90%);
  --fig-color: hsl(12, 100%, 17%);
}
}

*,
*::after,
*::before {
  box-sizing: border-box;
}
main {
  display: grid;
  place-content: center;
  height: 100vh;
  background-color: var(--bg-color);
}
main figure {
  min-width: 20em;
  background-color: var(--fig-color);
  color: var(--ft-color);
  width: 40vw;
  box-shadow: 0px 0px 15px var(--fig-color);
  padding: 1em 2em;
  aspect-ratio: 2/1;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 1.5em;
  place-items: center;
  font-family: sans-serif;
  grid-template-areas: "quote quote quote quote quote quote"
                        "quote quote quote quote quote quote"
                        "twitter . . author author update";  
}
main figure blockquote {
    font-weight: 300;
    margin: 0;
    padding-bottom: .5em;
    grid-area: quote;
  }
main figure figcaption {
    font-weight: 600;
    font-size: .9em;
    grid-area: author;
    padding-right: .5em;
  }

  main figure button {
    grid-area: update;
    padding: 1em 1.5em;
    font-size: .7em;
    width: 10em;   
    background-color: var(--bg-color);
    border: 0;
    box-shadow: 0 0 5px var(--bg-color);
    color: var(--ft-color);
    letter-spacing: 1px;
    cursor: pointer;
    transition: transform 500ms ease;
  }
  main figure button:hover,
  main figure button:focus {
    transform: scaleY(.95)
  }
main figure a {
  grid-area: twitter;
}
main figure a picture img {
  aspect-ratio: 1;
  width: 40px;
}

</style>