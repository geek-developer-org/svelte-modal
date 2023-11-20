<script>
  import { fade } from "svelte/transition";
  export let showLoginModal;

  let loginDialog;

  let account = null;
  let password = null;

  const isValidateString = (value, minlength, maxlength) => {
    if (value === null || typeof value !== "string") return false;
    if (/^\s*$/.test(value)) return false;
    return value.trim().length >= minlength && value.trim().length <= maxlength;
  };

  const onSubmit = (event) => {
    account = account.trim();
    password = password.trim();
    if (
      !isValidateString(account, 1, 160) ||
      !isValidateString(password, 1, 160)
    ) {
      event.stopPropagation();
      event.preventDefault();
      return false;
    }
    return true;
  };

  const onClose = () => {
    if (loginDialog.returnValue !== "submit") {
      account = null;
      password = null;
    }
    showLoginModal = false;
  };

  const onCancel = () => {
    loginDialog.returnValue = null;
  };

  $: if (loginDialog && !loginDialog.open && showLoginModal)
    loginDialog.showModal();
</script>

<dialog
  bind:this={loginDialog}
  transition:fade={{ delay: 250, duration: 300 }}
  on:submit={onSubmit}
  on:cancel={onCancel}
  on:close={onClose}
  class="modal"
>
  <form method="dialog">
    <h6>Login is required.</h6>
    <div class="echo-margin-bottom-sm">
      <input
        type="text"
        bind:value={account}
        size="20"
        maxlength="160"
        name="account"
        id="account"
        placeholder="account"
        required
        class="echo-input"
      />
    </div>
    <div class="echo-margin-bottom-sm">
      <input
        type="password"
        bind:value={password}
        size="20"
        maxlength="160"
        name="password"
        id="password"
        placeholder="password"
        required
        class="echo-input"
      />
    </div>
    <ul class="echo-buttons">
      <li class="echo-buttons-item">
        <button
          type="submit"
          value="submit"
          class="echo-button echo-button-style-a">OK</button
        >
      </li>
      <li class="echo-buttons-item">
        <button
          type="button"
          on:click|stopPropagation|self={() => {
            loginDialog.close(null);
          }}
          id="loginCancel"
          class="echo-button echo-button-style-a">Cancel</button
        >
      </li>
    </ul>
  </form>
</dialog>

{#if loginDialog && loginDialog.open && showLoginModal}
  <style>
    body {
      overflow: hidden;
      background-color: azure;
    }
  </style>
{/if}

<style>
  dialog[open] {
    border: 1px solid darkgrey;
    border-radius: 3px;
  }
</style>
