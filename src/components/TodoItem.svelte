<script>
  import Btn from "./Btn.svelte";
  import { createEventDispatcher, onMount } from "svelte";

  const dispatch = createEventDispatcher();

  let ref = undefined;
  export let status = false;
  export let content = "";
  export let autofocus = false;

  onMount(() => {
    if (autofocus) {
      ref.focus();
    }
  });

  const handleDeleteItem = () => {
    dispatch("deleteItem");
  };

  const saveTodoContent = () => {
    dispatch("saveTodoContent", { content });
  };

  const saveTodoStatus = () => {
    dispatch("saveTodoStatus", { status });
  };
</script>

<li class="todo-item">
  <input class="todo-item__status" type="checkbox" bind:checked={status} on:change={saveTodoStatus} />
  <input
    id="itemContent"
    class="todo-item__content"
    class:todo-item__content--done={status}
    type="text"
    bind:value={content}
    on:input={saveTodoContent}
    spellcheck="false"
    autocomplete="off"
    disabled={status}
    bind:this={ref}
  />
  <div class="todo-item__delete">
    <Btn btnXSmall={true} enabled={true} iconName="close" on:click={handleDeleteItem} />
  </div>
</li>

<style lang="scss">
  .todo-item {
    width: 100%;

    background-color: transparent;

    display: flex;
    align-items: center;
    gap: 1.2rem;

    transition: all 200ms ease-in;
    &:hover {
      background-color: rgba(0, 0, 0, 0.05);
    }

    &__status {
      margin-left: 3.2rem;

      cursor: pointer;
    }

    &__content {
      font-family: "Roboto", sans-serif;
      font-size: 1.2rem;

      width: 100%;

      border: transparent;
      background: transparent;
      outline: none;

      margin-right: auto;

      &--done {
        text-decoration: line-through;

        cursor: not-allowed;
      }
    }

    &__delete {
      justify-self: flex-end;
      opacity: 0;
      transition: opacity 200ms ease-in;
    }

    &:hover &__delete {
      opacity: 1;
    }
  }
</style>
