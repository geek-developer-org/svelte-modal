<script>
  import LoginModal from "./LoginModal.svelte";
  let showLoginModal = false;
</script>

<div class="echo-margin-sm">
  {#if !showLoginModal}
    <button
      on:click={() => {
        showLoginModal = true;
      }}
      class="echo-button echo-button-style-a"
    >
      Open
    </button>
  {/if}
</div>

<LoginModal bind:showLoginModal />
