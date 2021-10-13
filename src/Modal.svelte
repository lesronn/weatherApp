<script>
  export let showModal = false;
</script>

{#if showModal}
  <div class="backdrop" on:click|self>
    <div class="modal">
      <slot />
    </div>
  </div>
{/if}

<style>
  .backdrop {
    width: 100%;
    height: 100%;
    position: fixed;
    background: rgba(0, 0, 0, 0.6);
    display: flex;
    align-items: flex-start;
  }
  .modal {
    border-radius: 2px;
    width: 100%;
    margin: 5rem 1rem;
  }
</style>
