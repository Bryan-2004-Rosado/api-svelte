<script>
  import { onMount } from 'svelte';

  let characters = [];

  const query = `
  {
      characters(page: 1) {
          results {
              id
              name
              status
              species
              type
              gender
              image
          }
      }
  }`;

  onMount(async () => {
      const response = await fetch('https://rickandmortyapi.com/graphql', {
          method: 'POST',
          headers: {
              'Content-Type': 'application/json'
          },
          body: JSON.stringify({ query })
      });
      const data = await response.json();
      characters = data.data.characters.results;
  });
</script>

<style>
  body {
      font-family: 'Roboto', sans-serif;
      background-color: #202329;
      color: #ffffff;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      height: 100vh;
      overflow: auto;
  }

  .container {
      background-color: #3c3e44;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
      width: 90%;
      max-width: 1200px;
      text-align: center;
      margin-top: 20px;
  }

  h1 {
      font-size: 2em;
      margin-bottom: 20px;
      color: #61dafb;
  }

  .characters {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
  }

  .character {
      background-color: #282c34;
      border: 1px solid #61dafb;
      border-radius: 10px;
      padding: 15px;
      margin: 10px;
      width: 250px;
      text-align: left;
  }

  .character img {
      width: 100%;
      border-radius: 10px;
  }

  .character h2 {
      margin: 10px 0 5px;
      font-size: 1.5em;
      color: #61dafb;
  }

  .character p {
      margin: 5px 0;
      font-size: 1em;
      color: #9e9e9e;
  }
</style>

<div class="container">
  <h1>Rick and Morty Characters</h1>
  <div class="characters">
      {#each characters as character}
          <div class="character">
              <img src="{character.image}" alt="{character.name}">
              <h2>{character.name}</h2>
              <p>Status: {character.status}</p>
              <p>Species: {character.species}</p>
              <p>Type: {character.type || 'N/A'}</p>
              <p>Gender: {character.gender}</p>
          </div>
      {/each}
  </div>
</div>
