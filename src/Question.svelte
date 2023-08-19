<script>
  export let question
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
</script>

<h3>
  {@html question.question}
</h3>

{#each allAnswers as answer}
  <button>
    {@html answer.answer}
    {answer.correct}
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
