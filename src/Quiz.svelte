<script>
  import Question from './Question.svelte'
  let quiz = getQuiz()

  function pickAnswer(answer) {
    if (answer === correctAnswer) {
      result = 'Correct!'
    } else {
      result = 'Wrong!'
    }
    console.log('Answer Picked' + ' ' + answer)
  }

  async function getQuiz() {
    const res = await fetch(
      'https://opentdb.com/api.php?amount=10&category=10&difficulty=medium&type=multiple'
    )
    const quiz = await res.json()
    console.log(quiz)
    return quiz
  }

  function handleClick() {
    quiz = getQuiz()
  }
</script>

<div>
  <button on:click={handleClick}>Get New Questions!</button>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question}
      <Question {question} />
    {/each}
  {:catch error}
    <p>{error.message}</p>
  {/await}
</div>

<style>
  button {
    margin: 5px;
    padding: 5px;
    border-radius: 5px;
    background-color: #eee;

    &:hover {
      background-color: #ddd;
    }
  }
</style>
