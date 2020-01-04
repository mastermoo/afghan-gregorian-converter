<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let value;
  export let placeholder;
  export let totalDigits;
  export let max;

  let focused = false;
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
      (textSelected() || $event.target.textContent.length < totalDigits)
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

  function onFocus($event) {
    selectAll($event);
    focused = true;
  }

  function handleInput($event) {
    const upcomingValue = parseInt(`${value}${$event.key}`);
    if (
      validateField($event) &&
      (textSelected() || !max || !value || upcomingValue <= max)
    ) {
      if (
        !textSelected() &&
        $event.target.textContent.length + 1 == totalDigits
      ) {
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
    border-bottom: 1px solid rgba(0, 0, 0, 0.2);
    display: flex;
  }
  [contenteditable="true"] {
    outline: none;
  }
  .field.active {
    border-bottom-color: #039130;
  }
</style>

<span class="field" class:active={focused}>
  <span
    bind:this={field}
    contenteditable="true"
    bind:textContent={value}
    on:keypress={handleInput}
    on:input
    on:focus={onFocus}
    on:blur={() => (focused = false)} />
  {#each Array(totalDigits - value.toString().length) as i}
    <span on:click={() => field.focus()} style="color: #999">
      {placeholder}
    </span>
  {/each}
</span>
