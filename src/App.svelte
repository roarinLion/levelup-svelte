<script>
  import Quiz from './Quiz.svelte'
</script>

<main>
  <h1>Hello World!</h1>
  <Quiz />
</main>

<style>
  :global(body) {
    background-color: black;
  }
  :global(h1, h2, h3, h4, h5, h6, p) {
    color: white;
  }
</style>
