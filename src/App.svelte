<script>
  let we_love_each_other = "That through which we perceive each other";
  let we_know_each_other = [
    "I",
    "see",
    "you",
    "me",
    "and",
    "know",
    "understand",
    "ourselves",
    "myself",
    "yourself",
    "each",
    "other",
    "through",
    "which",
    "we",
    "perceive",
  ];

  let hands = 4;
  let minds = 2;
  let hearts = 4;

  let currentWordIndex = 0;
  let currentKnowIndex = 0;
  let currentInnerIndex = randomInt(we_know_each_other.length - 1);

  let windowStates = ["window.png", "window-open-in.png"];
  let currentWindowIndex = 0;
  let isOtherClicked = false;
  let isInnerClicked = false;

  function splitIntoWords(str) {
    return str.split(" ");
  }

  function randomInt(n) {
    return Math.floor(Math.random() * (n + 1));
  }

  function cycleOtherWord() {
    currentKnowIndex = randomInt(we_know_each_other.length - 1);
  }

  function cycleInnerWord() {
    currentInnerIndex = randomInt(we_know_each_other.length - 1);
  }

  function handleOtherDown() {
    isOtherClicked = true;
    cycleOtherWord();
  }

  function handleOtherUp() {
    isOtherClicked = false;
  }

  function handleInnerDown() {
    isInnerClicked = true;
    cycleInnerWord();
  }

  function handleInnerUp() {
    isInnerClicked = false;
  }

  function cycleWindow() {
    currentWindowIndex = (currentWindowIndex + 1) % windowStates.length;
  }

  let my = randomInt(19);
  $: words = splitIntoWords(we_love_each_other);

  // Only keep interval for self section
  let interval;

  function startWordCycles() {
    interval = setInterval(() => {
      currentWordIndex = (currentWordIndex + 1) % words.length;
    }, 1000);
  }

  // Start the animation when component mounts
  import { onMount, onDestroy } from "svelte";

  onMount(() => {
    startWordCycles();
  });

  onDestroy(() => {
    if (interval) clearInterval(interval);
  });
</script>

<main>
  <div class="our eyes">
    <div class="our self">
      <span class="voice">{words[currentWordIndex]}</span>
    </div>

    <div class="our second self">
      <span class="voice">{words[currentWordIndex]}</span>
    </div>

    <div
      class="our other {isOtherClicked ? 'clicked' : ''}"
      on:mousedown={handleOtherDown}
      on:mouseup={handleOtherUp}
      on:mouseleave={handleOtherUp}
      on:touchstart={handleOtherDown}
      on:touchend={handleOtherUp}
    >
      <span class="voice">{we_know_each_other[currentKnowIndex]}</span>
    </div>

    <div
      class="our inner {isInnerClicked ? 'clicked' : ''}"
      on:mousedown={handleInnerDown}
      on:mouseup={handleInnerUp}
      on:mouseleave={handleInnerUp}
      on:touchstart={handleInnerDown}
      on:touchend={handleInnerUp}
    >
      <span class="voice">{we_know_each_other[currentInnerIndex]}</span>
    </div>

    {#each Array(hands) as _, i}
      <div class="our hands o-{i}">
        <div class="marquee-container">
          <div class="marquee-content speed-{i}">
            {#each words as word}
              <span class="voice">{word + " "}</span>
            {/each}
            {#each words as word}
              <span class="voice">{word + " "}</span>
            {/each}
          </div>
        </div>
      </div>
    {/each}

    {#each Array(hands) as _, i}
      <div class="our tongues o-{i}">
        <div class="marquee-container">
          <div class="marquee-content speed-{i}">
            {#each words as word}
              <span class="voice">{word + " "}</span>
            {/each}
            {#each words as word}
              <span class="voice">{word + " "}</span>
            {/each}
          </div>
        </div>
      </div>
    {/each}

    <div class="our souls">
      <div class="marquee-container">
        <div class="marquee-content souls-marquee">
          {#each words as word}
            <span class="voice">{word + " "}</span>
          {/each}
          {#each words as word}
            <span class="voice">{word + " "}</span>
          {/each}
        </div>
      </div>
    </div>

    {#each Array(minds) as _, i}
      <div class="our minds o-{i}">
        <div class="marquee-container">
          <div class="marquee-content smooth">
            {#each words as word}
              <span class="voice">{word + " "}</span>
            {/each}
            {#each words as word}
              <span class="voice">{word + " "}</span>
            {/each}
          </div>
        </div>
      </div>
    {/each}

    {#each Array(hearts) as _, i}
      <div class="our hearts o-{i}">
        <div class="marquee-container vertical">
          <div class="marquee-content vertical">
            {#each words as word}
              <span class="voice">{word}</span>
            {/each}
            {#each words as word}
              <span class="voice">{word}</span>
            {/each}
          </div>
        </div>
      </div>
    {/each}

    {#each Array(hearts) as _, i}
      <div class="our mouths o-{i}">
        <div class="marquee-container vertical">
          <div class="marquee-content vertical">
            {#each words as word}
              <span class="voice">{word}</span>
            {/each}
            {#each words as word}
              <span class="voice">{word}</span>
            {/each}
          </div>
        </div>
      </div>
    {/each}
    <div
      class="the-window window-{currentWindowIndex}"
      on:click={cycleWindow}
      on:touchstart={cycleWindow}
    >
      <img src="/images/{windowStates[currentWindowIndex]}" alt="window" />
    </div>
  </div>
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    margin: 0;
    padding: 0;
  }
  .our .our {
    background-color: white;
  }
  .our {
    display: flex;
    align-items: center;
    height: 100%;
  }
  .our.self {
    /* border: solid 1px red; */
    grid-column: 1/2;
    grid-row: 2/3;
    overflow: hidden;
    justify-content: center;
    box-sizing: border-box;
    border-radius: 5px;
    box-shadow:
      inset rgb(213 213 213) -1px -1px 5px 1px,
      inset rgb(127 127 127) 2px 2px 2px 0px;
  }
  .our.second.self {
    grid-column: 8/9;
    box-sizing: border-box;
    border-radius: 5px;
    box-shadow:
      inset rgb(213 213 213) -1px -1px 5px 1px,
      inset rgb(127 127 127) 2px 2px 2px 0px;
  }
  .our.souls {
    grid-column: 2 / 3;
    grid-row: 3 / 7;
    overflow: hidden;
    box-sizing: border-box;
    border-radius: 5px;
    box-shadow:
      inset rgb(213 213 213) -1px -1px 11px 3px,
      inset rgb(127 127 127) 2px 2px 2px 0px;
    background-color: #e9e8e8;
  }

  .our.souls .marquee-content {
    display: flex;
    flex-direction: column;
    align-items: center;

    /* display: inline-block; */
    white-space: nowrap;
    align-items: center;
    height: 100%;
    transform: rotate(-90deg);
    animation: souls-marquee 20s steps(20) infinite;
  }

  .our.souls .marquee-content,
  .our.souls .marquee-container {
    height: 100%;
  }
  .our.souls .voice {
    /* transform: rotate(-90deg); */
    display: inline-block;
    text-align: center;
    /* height: 100%; */
    font-size: 50px;
  }
  .our.other,
  .our.inner {
    grid-column: 1/2;
    grid-row: 7/8;
    overflow: hidden;

    justify-content: center;
    cursor: pointer;
    user-select: none;

    box-sizing: border-box;
    border-radius: 5px;
    box-shadow:
      inset #303030 -1px -4px 6px 2px,
      rgb(0 0 0 / 10%) 1px 1px 2px 2px;
    background-color: #929292;
    transition: all 0.2s ease;
  }

  .our.other.clicked,
  .our.inner.clicked {
    background-color: #666;
    box-shadow:
      inset #303030 2px 3px 6px 2px,
      rgb(0 0 0 / 10%) 1px 1px 2px 2px;
    transform: translateY(1px);
  }
  .our.other .voice,
  .our.inner .voice {
    background-color: white;
    padding: 1px 1px 0 1px;
    border-radius: 1px;
    overflow: hidden;
  }
  .our.inner {
    grid-column: 8/9;
  }
  .our.self .voice {
    font-size: 20px;
  }
  .our.inner .voice,
  .our.other .voice {
    font-size: 10px;
  }
  .our.eyes {
    width: 100%;
    height: 100%;
    display: grid;
    gap: 20px;
    grid-template-rows: repeat(8, 1fr);
    grid-template-columns: repeat(8, 1fr);

    width: calc(100% - 40px);
    height: calc(100% - 40px);
    margin: auto;
    padding-top: 20px;
  }
  .our.hands,
  .our.tongues {
    /* border: solid 1px red; */
    grid-column: span 2;
    grid-row: span 1;
    white-space: nowrap;
    overflow: hidden;
    position: relative;
    /* border: solid black 1px; */
    box-sizing: border-box;
    border-radius: 5px;
    box-shadow:
      rgb(191 191 191) 2px 4px 4px 5px,
      inset rgb(127 127 127) 6px 5px 5px 0px;

    background-color: white;
    padding-top: 6px;
  }
  .our.hands.o-0 {
    grid-column: 1/3;
    grid-row: 1;
  }
  .our.hands.o-1 {
    grid-column: 3/5;
    grid-row: 1;
  }
  .our.hands.o-2 {
    grid-column: 5/7;
    grid-row: 1;
  }
  .our.hands.o-3 {
    grid-column: 7/9;
    grid-row: 1;
  }

  .our.tongues.o-0 {
    grid-column: 1/3;
    grid-row: 8/9;
  }
  .our.tongues.o-1 {
    grid-column: 3/5;
    grid-row: 8/9;
  }
  .our.tongues.o-2 {
    grid-column: 5/7;
    grid-row: 8/9;
  }
  .our.tongues.o-3 {
    grid-column: 7/9;
    grid-row: 8/9;
  }

  .our.minds {
    /* border: solid 1px blue; */
    grid-row: span 1;
    white-space: nowrap;
    overflow: hidden;
  }
  .our.minds.o-0 {
    grid-column: 2/8;
    grid-row: 2;
  }
  .our.minds.o-1 {
    grid-column: 2/8;
    grid-row: 7;
  }
  .our.minds .voice {
    font-size: 50px;
  }
  span.voice {
    font-size: 90px;
    font-weight: 500;
    text-transform: uppercase;
    font-family: "Helvetica", sans-serif;
  }

  .our.hearts,
  .our.mouths {
    /* border: solid 1px blue; */
    grid-row: span 1;
    white-space: nowrap;
    overflow: hidden;
  }
  .our.hearts .voice,
  .our.mouths .voice {
    display: block;
  }
  .our.hearts .voice {
    text-align: center;
    font-size: 10px;
    margin: 0;
  }
  .our.minds,
  .our.hearts {
    box-sizing: border-box;
    border-radius: 5px;
    box-shadow:
      inset rgb(213 213 213) -1px -1px 11px 3px,
      inset rgb(127 127 127) 2px 2px 2px 0px;

    background-color: #e9e8e8;
  }
  .our.mouths {
    box-sizing: border-box;
    border-radius: 5px;
    box-shadow:
      inset rgb(213 213 213) -1px -1px 11px 3px,
      inset rgb(127 127 127) 2px 2px 2px 0px;

    background-color: #e9e8e8;
  }
  .our.mouths .voice {
    font-size: 50px;
    text-align: center;
  }
  .our.hearts.o-0 {
    grid-column: 1/2;
    grid-row: 3;
  }
  .our.hearts.o-1 {
    grid-column: 1/2;
    grid-row: 4;
  }
  .our.hearts.o-2 {
    grid-column: 1/2;
    grid-row: 5;
  }
  .our.hearts.o-3 {
    grid-column: 1/2;
    grid-row: 6;
  }

  .our.mouths.o-0 {
    grid-column: 7/9;
    grid-row: 3;
  }
  .our.mouths.o-1 {
    grid-column: 7/9;
    grid-row: 4;
  }
  .our.mouths.o-2 {
    grid-column: 7/9;
    grid-row: 5;
  }
  .our.mouths.o-3 {
    grid-column: 7/9;
    grid-row: 6;
  }

  .the-window {
    grid-row: 3 / 7;
    grid-column: 3 / 7;
    height: 100%;
    width: 100%;

    box-shadow: inset black -9px 15px 31px;
    border-radius: 5px;
    overflow: hidden;
    background-color: #fff;
    transition: background-color 0.2s ease;
  }

  .window-0 {
    background-color: #fff;
  }
  .window-1 {
    background-color: #000;
  }
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    /* mix-blend-mode: multiply; */
  }

  .marquee-container {
    width: 100%;
    overflow: hidden;
    position: relative;
  }

  .marquee-content {
    display: inline-block;
    white-space: nowrap;
  }

  .marquee-content.speed-0 {
    animation: marquee 80s steps(20) infinite; /* 4x slower (20s * 4) */
  }

  .marquee-content.speed-1 {
    animation: marquee 60s steps(20) infinite; /* 3x slower (20s * 3) */
  }

  .marquee-content.speed-2 {
    animation: marquee 40s steps(20) infinite; /* 2x slower (20s * 2) */
  }

  .marquee-content.speed-3 {
    animation: marquee 20s steps(20) infinite; /* original speed */
  }

  .marquee-content.smooth {
    animation: marquee 20s linear infinite;
  }

  @keyframes marquee {
    0% {
      transform: translateX(0);
    }
    100% {
      transform: translateX(-50%);
    }
  }

  .marquee-container.vertical {
    height: 100%;
    overflow: hidden;
  }

  .marquee-content.vertical {
    display: block;
    animation: marquee-vertical 20s steps(20) infinite;
  }

  @keyframes marquee-vertical {
    0% {
      transform: translateY(0);
    }
    100% {
      transform: translateY(-50%);
    }
  }
  .our.hearts .voice {
    display: block;
    margin: 10px 0;
  }

  .our.mouths .voice {
    display: block;
    margin: 30px 0;
  }

  .our.self .voice {
    transition: opacity 0.2s ease;
  }

  .our.other .voice {
    transition: opacity 0.2s ease;
  }

  @keyframes souls-marquee {
    0% {
      transform: translateX(0) rotate(-90deg);
    }
    100% {
      transform: translateX(-100%) rotate(-90deg);
    }
  }
</style>
