<script>
  import Btn from "./Btn.svelte";
  import autosize from "autosize";
  import { createEventDispatcher, onMount } from "svelte";

  const dispatch = createEventDispatcher();

  let ref = undefined;
  export let status = false;
  export let content = "";
  export let autofocus = false;
  export let dragged = false;
  export let hovered = false;
  export let draggable = true;

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

<li
  class="todo-item"
  {draggable}
  class:dragged
  class:hovered
  on:dragstart
  on:drop
  on:dragenter
  ondragover="return false"
>
  <img src="assets/icons/icon-dnd.svg" alt="Drag and Drop" class="todo-item__dnd-icon" class:draggable />
  <input class="todo-item__status" type="checkbox" bind:checked={status} on:change={saveTodoStatus} />
  <textarea
    id="itemContent"
    class="todo-item__content"
    class:todo-item__content--done={status}
    type="text"
    bind:value={content}
    on:input={(event) => {
      autosize(event.target);
      saveTodoContent();
    }}
    spellcheck="false"
    placeholder="Todo"
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

    &__dnd-icon {
      width: 2.4rem;
      height: 2.4rem;

      display: none;
      opacity: 0;
    }

    &__status {
      margin-left: 1.2rem;

      cursor: pointer;
    }

    &__content {
      font-family: "Roboto", sans-serif;
      font-size: 1.2rem;
      line-height: 1;

      width: 100%;

      border: transparent;
      background: transparent;
      outline: none;

      resize: none;
      padding-top: 1.4rem;

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

  .draggable {
    display: inline-block;
    opacity: 1;
  }
  .dragged {
    opacity: 20%;
  }
  .hovered {
    box-shadow: 0 5px 2rem 2px rgba(0, 0, 0, 0.1);
  }
</style>
