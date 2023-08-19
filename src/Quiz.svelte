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
  import Modal from './Modal.svelte'
  import { score } from './store'

  /**
   * The index of the currently active question.
   * @type {number}
   */
  let activeQuestion = 0

  /**
   * Fetches the quiz data.
   * @type {Promise<Object>}
   */
  let quiz = getQuiz()
  let isModalOpen = false

  function closeModal() {
    isModalOpen = false
    resetQuiz()
  }

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
    score.set(0)
    quiz = getQuiz()
    activeQuestion = 0
  }

  // reactive statement
  $: if ($score > 2) {
    isModalOpen = true
  }
  // reactive statement
  $: questionNumber = activeQuestion + 1
</script>

<!-- Main content of the component -->
<div>
  <!-- Button to start a new quiz -->
  <button on:click|once={resetQuiz}>Start New Quiz!</button>

  <!-- Displaying the user's score (static as 0 for now) -->
  <h4>My Score: {$score}</h4>

  <!-- Displaying the current question number -->
  <h4>Question: #{questionNumber}</h4>

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

{#if isModalOpen}
  <!-- content here -->
  <Modal on:close={closeModal}>
    <h1>You Won Roarin's Mythology Quiz!</h1>
    <p>Your score is {score}</p>
    <button
      class="startOverButton"
      on:click={closeModal}>Start Over</button
    >
  </Modal>
{/if}

<style>
  /* This keeps the  questions on top of eachother fading in and out */
  .fade-wrapper {
    position: absolute;
  }

  .startOverButton {
    background: none;
    border: none;
    cursor: pointer;
    color: white;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    text-align: center;
  }
</style>
