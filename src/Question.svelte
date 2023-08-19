<script>
  /**
   * Represents a single quiz question.
   * @typedef {Object} Question
   * @property {string} question - The text of the question.
   * @property {string} correct_answer - The correct answer for the question.
   * @property {string[]} incorrect_answers - An array of incorrect answers.
   */

  /**
   * The question data for the quiz.
   * @type {Question}
   */
  export let question

  /**
   * Indicates if the selected answer is correct.
   * @type {boolean}
   */
  let isCorrect

  /**
   * Indicates if the user has answered the question.
   * @type {boolean}
   */
  let isAnswered = false

  /**
   * Maps the incorrect answers to an array of objects with answer text and correctness status.
   * @type {Array<{ answer: string, correct: boolean }>}
   */
  let answers = question.incorrect_answers.map((answer) => {
    return {
      answer,
      correct: false,
    }
  })

  /**
   * Combines the correct answer with the incorrect answers and shuffles them.
   * @type {Array<{ answer: string, correct: boolean }>}
   */
  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true,
    },
  ]
  shuffle(allAnswers)

  /**
   * Shuffles the elements of an array.
   * @param {Array} array - The array to shuffle.
   */
  function shuffle(array) {
    array.sort(() => Math.random() - 0.5)
  }

  /**
   * Checks if the selected answer is correct and updates the UI accordingly.
   * @param {boolean} correct - Indicates if the selected answer is correct.
   */
  function checkQuestion(correct) {
    isCorrect = correct
    isAnswered = true
  }
</script>

<!-- Displays the question text -->
<h3>
  {@html question.question}
</h3>

<!-- Displays feedback based on the user's answer -->
{#if isAnswered}
  <h4>
    {#if isCorrect}
      Correct Answer 🎉
    {:else}
      Wrong Answer 😢
    {/if}
  </h4>
{/if}

<!-- Displays the answer options as buttons -->
{#each allAnswers as answer}
  <button on:click={() => checkQuestion(answer.correct)}>
    {@html answer.answer}
  </button>
{/each}

<style>
  /* Styles for the answer buttons */
  button {
    margin: 5px;
    padding: 5px;
    border-radius: 5px;
    background-color: #eee;
    cursor: pointer;
    &:hover {
      background-color: #ddd;
    }
  }
</style>
