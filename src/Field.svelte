<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let value;
  export let placeholder;
  export let max;

  let field;

  function isDigit($event) {
    return $event.charCode >= 48 && $event.charCode <= 57;
  }

  function textSelected() {
    return window.getSelection().toString();
  }

  function validateField($event) {
    return (
      isDigit($event) &&
      (textSelected() || $event.target.textContent.length < max)
    );
  }

  export function focus() {
    field.focus();
  }

  function selectAll($event) {
    window.setTimeout(function() {
      const range = document.createRange();
      range.selectNodeContents($event.target);
      const sel = window.getSelection();
      sel.removeAllRanges();
      sel.addRange(range);
    }, 1);
  }

  function handleInput($event) {
    if (validateField($event)) {
      if ($event.target.textContent.length + 1 == max) {
        setTimeout(() => {
          dispatch("next", { $event });
        }, 25);
      }
    } else {
      $event.preventDefault();
    }
  }
</script>

<style>
  .field {
    border-bottom: 1px solid transparent;
    display: flex;
  }
  [contenteditable="true"] {
    outline: none;
  }
  .field:focus {
    border-bottom-color: inherit;
  }
</style>

<span class="field">
  <span
    type="text"
    pattern="\d*"
    bind:this={field}
    contenteditable="true"
    bind:textContent={value}
    on:keypress={handleInput}
    on:input
    on:focus={selectAll} />
  {#each Array(max - value.toString().length) as i}
    <span on:click={() => field.focus()} style="color: #999">
      {placeholder}
    </span>
  {/each}
</span>
