<script>
  import { fade } from 'svelte/transition'
</script>

<div
  out:fade={{ duration: 250 }}
  in:fade={{ duration: 250 }}
  class="wrapper"
>
  <div class="modal">
    <!-- <slot>optional fallback</slot> -->

    <slot />
  </div>
</div>

<style>
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
  }
  .modal {
    position: fixed;
    background: rgb(31, 31, 31);
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
  }
</style>
