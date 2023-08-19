<script>
  /**
   * @module QuizApp
   */

  /**
   * The Quiz component responsible for rendering individual quiz questions and handling user interactions.
   * @type {import('./Quiz.svelte').SvelteComponent}
   */
  import Quiz from './Quiz.svelte'
</script>

<!-- Main content of the component -->
<main>
  <!-- Displaying the title of the quiz -->
  <h1>Roarin's Mythology Quiz</h1>

  <!-- Embedding the Quiz component -->
  <Quiz />
</main>

<style>
  /* Setting the global background color for the body element */
  :global(body) {
    background-color: black;
  }

  /* Setting the global text color for header and paragraph elements */
  :global(h1, h2, h3, h4, h5, h6, p) {
    color: white;
  }
</style>
