<script>
  import Pusher from "pusher-js";

  import "@fontsource/inter";
  let count = 0;
  let val = "";
  let quote = "";
  let pusher = new Pusher("76f8d754429d0de93874", { cluster: "ap2" });
  let cacheChannel = pusher.subscribe("cache-quote");

  cacheChannel.bind("newQuote", (data) => {
    quote = data;
  });

  cacheChannel.bind("pusher:cache_miss", function () {
    fetch("/cachemiss", { method: "POST" });
  });

  cacheChannel.bind("pusher:subscription_count", (data) => {
    count = data.subscription_count;
  });
  async function quoteFn() {
    if (val.trim() !== "") {
      const rawResponse = await fetch("/quote", {
        method: "POST",
        body: JSON.stringify({ val }),
        headers: {
          "content-type": "application/json",
        },
      });
    }

    const content = await rawResponse;

    console.log(content);
  }
</script>

<main>
  <a href="https://github.com/VanshajPoonia/announcerrr"><h1>Source Code :&#41</h1></a>
  <h2>"{quote}"</h2>
  <p>&#40 This is a website to announce something to a large number of people. <br>
    Helpful for schools, colleges or even a small group of friends. &#41
  </p>
  <form on:submit|preventDefault on:submit={() => (val = "")}>
    <!-- svelte-ignore a11y-autofocus -->
    <input
      autofocus
      type="text"
      bind:value={val}
      required
      placeholder="I wanna accounce..."
    />
    <button on:click={quoteFn}>Announce 🚀</button>
  </form>
  
  
  <p id="count">Number of People who have seen it : {count}</p>
</main>

<style>
  :global(body) {
    background: #06edb4;
    background: -webkit-linear-gradient(to left, #0993dd, #ff5acd);
    background: #8b49ff;
  }
  main {
    font-family: Inter, Montserrat, "sans-serif";
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    /* height: 60vh; */
    align-items: center;
    margin: 10vh 0;
    overflow: hidden;
  }
  input {
    font-size: 1.5rem;
    border: none;
    max-width: 90vw;
    margin: 40px;
    background-color: transparent;
    outline: none;
    text-align: center;
  }
  p {
    margin-top: 10px;
    font-size: 20px;
    color: #fcfcfc;
    text-align: center;
  }
  h2 {
    font-size: 4rem;
    max-width: 90vw;
    word-wrap: break-word;
    font-style: italic;
    margin-top: 90px;

  }
  main > form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    justify-items: center;
    align-items: center;
  }
  button {
    width: 35%;
    padding: 1em 1em;
    border: none;
    border-radius: 10px;
    font-size: 1rem;
    margin-top: 2vh;
    margin-bottom: 30px;
    background: #ffca58;
    background: -webkit-linear-gradient(to left, #ffca58, #8b49ff);
    background: #fcfcfc;
    box-shadow: 12px 9px 21px -1px rgba(0, 0, 0, 0.25);
  }
  button:active {
    box-shadow: inset 12px 9px 21px -1px rgba(0, 0, 0, 0.25);
  }
  h1 {
    position: fixed;
    bottom: 20px;
    right: 60px;
    font-size: 1.5rem;
  }
  a { color: inherit; }
  @media screen and (max-width: 800px) {
    h1 {
      display: none;
    }
    #count {
      position: fixed;
      bottom: 0;
    }
  }
</style>
