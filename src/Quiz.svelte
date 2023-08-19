<script>
  /**
   * @module QuizComponent
   */

  /**
   * The Question component responsible for rendering individual quiz questions and handling user interactions.
   * @type {import('./Question.svelte').SvelteComponent}
   */
  import Question from './Question.svelte'
  import { fly } from 'svelte/transition'

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
        <div
          in:fly={{ x: 100, duration: 200 }}
          out:fly={{ x: -200, duration: 200 }}
          class="fade-wrapper"
        >
          <!-- Rendering the active question -->
          <Question
            {addToScore}
            {nextQuestion}
            {question}
          />
        </div>
      {/if}
    {/each}
  {:catch error}
    <!-- Displaying any errors that occur while fetching the quiz data -->
    <p>{error.message}</p>
  {/await}
</div>

<style>
  /* This keeps the  questions on top of eachother fading in and out */
  .fade-wrapper {
    position: absolute;
  }
</style>
