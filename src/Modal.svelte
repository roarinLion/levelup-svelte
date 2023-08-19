<script>
  import { fade, fly } from 'svelte/transition'
  import { createEventDispatcher } from 'svelte'

  const dispatch = createEventDispatcher()
</script>

<div
  transition:fade={{ duration: 200 }}
  class="wrapper"
>
  <div
    transition:fly={{ y: -100 }}
    class="modal"
  >
    <button
      on:click={() => {
        dispatch('close')
      }}
    >
      Close
    </button>
    <!-- <slot>optional fallback</slot> -->

    <slot />
  </div>
</div>

<style lang="scss">
  .wrapper {
    position: fixed;
    display: flex;
    align-items: center;
    justify-content: center;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(8, 8, 8, 0.6);
    .modal {
      text-align: center;
      position: fixed;
      background: rgb(31, 31, 31);
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
    }
  }
</style>
