<script>
  export let question
  let isCorrect
  let isAnswered = false

  let answers = question.incorrect_answers.map((answer) => {
    return {
      answer,
      correct: false,
    }
  })
  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true,
    },
  ]
  shuffle(allAnswers)

  function shuffle(array) {
    array.sort(() => Math.random() - 0.5)
  }

  function checkQuestion(correct) {
    isCorrect = correct
    isAnswered = true
  }
</script>

<h3>
  {@html question.question}
</h3>
{#if isAnswered}
  <h4>
    {#if isCorrect}
      Correct Answer 🎉
    {:else}
      Wrong Answer 😢
    {/if}
  </h4>{/if}
{#each allAnswers as answer}
  <button on:click={() => checkQuestion(answer.correct)}>
    {@html answer.answer}
  </button>
{/each}

<style>
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
