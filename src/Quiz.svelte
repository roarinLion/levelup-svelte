<script>
  /**
   * @module QuizComponent
   */

  /**
   * The Question component responsible for rendering individual quiz questions and handling user interactions.
   * @type {import('./Question.svelte').SvelteComponent}
   */
  import Question from './Question.svelte'

  /**
   * The index of the currently active question.
   * @type {number}
   */
  let activeQuestion = 0

  let score = 0

  /**
   * Fetches the quiz data.
   * @type {Promise<Object>}
   */
  let quiz = getQuiz()

  /**
   * Fetches quiz data from the Open Trivia Database API.
   * @async
   * @function
   * @returns {Promise<Object>} The fetched quiz data.
   */
  async function getQuiz() {
    const res = await fetch(
      'https://opentdb.com/api.php?amount=10&category=20&difficulty=hard&type=multiple'
    )
    const quiz = await res.json()
    return quiz
  }

  /**
   * Handles the click event to start a new quiz.
   * @function
   */

  function nextQuestion() {
    activeQuestion = activeQuestion + 1
  }

  function resetQuiz() {
    score = 0
    quiz = getQuiz()
    activeQuestion = 0
  }
  function addToScore() {
    score = score + 1
  }
</script>

<!-- Main content of the component -->
<div>
  <!-- Button to start a new quiz -->
  <button on:click={resetQuiz}>Start New Quiz!</button>

  <!-- Displaying the user's score (static as 0 for now) -->
  <h4>My Score: {score}</h4>

  <!-- Displaying the current question number -->
  <h4>Question: #{activeQuestion + 1}</h4>

  <!-- Loading and displaying quiz data -->
  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <!-- Rendering the active question -->
        <Question
          {addToScore}
          {nextQuestion}
          {question}
        />
      {/if}
    {/each}
  {:catch error}
    <!-- Displaying any errors that occur while fetching the quiz data -->
    <p>{error.message}</p>
  {/await}
</div>

<style>
  /* Add any styles here if needed */
</style>
