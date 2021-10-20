<script>
  import Btn from "./Btn.svelte";
  import TodoItem from "./TodoItem.svelte";
  import NoteColorPicker from "./NoteColorPicker.svelte";
  import { createEventDispatcher } from "svelte";
  import { flip } from "svelte/animate";
  import { fade, fly } from "svelte/transition";

  export let draggable = true;
  export let content = "";
  export let title = "";
  export let color = "#fff";
  export let todos = [];

  export let hovered = false;
  export let dragged = false;

  let selected = false;
  let showMenu = false;
  let colorPicker = false;

  const dispatch = createEventDispatcher();

  const handleChangeColor = (event) => {
    color = event.detail.selectedColor;
    dispatch("changeColor", { color });
  };

  const handleSelection = () => {
    selected = !selected;
    dispatch("selection", {
      selectionStatus: selected,
    });
  };

  const handleDeletion = () => {
    dispatch("delete");
  };

  const heandleOpenNote = (event) => {
    if (event.target.id === "note" || event.target.id === "title" || event.target.id === "content") {
      dispatch("openNote");
    }
  };

  const handleSaveTodoContent = (event, itemIdx) => {
    let toUpdate = todos[itemIdx];
    toUpdate.content = event.detail.content;
    todos = todos;
    dispatch("updateTodoContent", { itemIdx, content: event.detail.content });
  };

  const handleSaveTodoStatus = (event, itemIdx) => {
    let toUpdate = todos[itemIdx];
    toUpdate.status = event.detail.status;
    todos = todos;
    dispatch("updateTodoStatus", { itemIdx, status: event.detail.status });
  };

  const handleItemDelete = (event, itemIdx) => {
    todos.splice(itemIdx, 1);
    todos = todos;
    if (todos.length === 0) {
      dispatch("deleteTodoNote");
    } else {
      dispatch("todoItemDelete", { todos });
    }
  };
</script>

<article
  id="note"
  style="background-color: {color}; "
  class="note"
  class:note--selected={selected}
  class:hovered
  class:dragged
  {draggable}
  on:dragstart
  on:dragenter
  on:drop
  on:pointerleave={() => (showMenu = false)}
  on:click={(event) => heandleOpenNote(event)}
  ondragover="return false"
>
  <img class="note__select" src="assets/icons/icon-select.svg" alt="Select" on:click={handleSelection} />
  <div id="title" class="note__header">
    <h3 class="note__title">{title}</h3>
    <div class="note__btn-1">
      <Btn iconName="pin" btnSmall={true} />
    </div>
  </div>
  <p id="content" class="note__content">
    {#if todos.length > 0}
      {#each todos as todoItem, idx (todoItem)}
        <div class="animation" animate:flip in:fade|local out:fly|local={{ x: 50 }}>
          <TodoItem
            status={todoItem.status}
            content={todoItem.content}
            draggable={false}
            on:saveTodoContent={(event) => handleSaveTodoContent(event, idx)}
            on:saveTodoStatus={(event) => handleSaveTodoStatus(event, idx)}
            on:deleteItem={(event) => handleItemDelete(event, idx)}
          />
        </div>
      {/each}
    {:else}
      {content}
    {/if}
  </p>
  <p />
  <div class="note__controls">
    <div class="note__btn-2">
      <Btn iconName="bellPlus" btnXSmall={true} />
    </div>
    <div class="note__btn-3">
      <Btn iconName="userPlus" btnXSmall={true} />
    </div>
    <div class="note__btn-4" on:pointerenter={() => (colorPicker = true)} on:pointerleave={() => (colorPicker = false)}>
      <NoteColorPicker noteColor={color} isVisible={colorPicker} on:colorChange={(event) => handleChangeColor(event)} />
      <Btn iconName="palette" btnXSmall={true} enabled={true} />
    </div>
    <div class="note__btn-5">
      <Btn iconName="image" btnXSmall={true} />
    </div>
    <div class="note__btn-6">
      <Btn iconName="archive" btnXSmall={true} />
    </div>
    <div class="note__btn-7">
      <Btn
        iconName="dots"
        btnXSmall={true}
        enabled={true}
        on:click={() => {
          showMenu = !showMenu;
        }}
      />
      <ul class="note__menu {showMenu ? 'visible' : ''}">
        <li class="note__menu-item" on:click={handleDeletion}>
          <span>Elimina nota</span>
        </li>
      </ul>
    </div>
  </div>
</article>

<style lang="scss">
  .note {
    align-self: flex-start;

    max-width: 24rem;

    border-radius: 8px;
    border: 1px solid #e0e0e0;
    padding: 1rem 1rem 0.4rem 1rem;
    box-shadow: none;
    background: transparent;

    cursor: pointer;
    border: 2px solid var(--color-gray-light-2);

    position: relative;

    display: flex;
    flex-direction: column;

    transition: all 200ms ease-in;

    ::selection {
      background: transparent;
    }
    &:hover {
      box-shadow: 0 1px 2px 0 rgb(60 64 67 / 30%), 0 1px 3px 1px rgb(60 64 67 / 15%);
    }

    &__header {
      display: flex;
      justify-content: space-between;
      align-items: center;

      margin-bottom: 1.2rem;
    }
    &__title {
      font-size: 2rem;
      font-weight: 400;
      color: var(--color-gray-dark-2);

      text-overflow: ellipsis;
      overflow: hidden;
    }

    &__content {
      font-family: "Roboto", sans-serif;
      font-size: 1.6rem;
      font-weight: 400;
      color: var(--color-gray-dark-1);
      word-wrap: break-word;

      margin-bottom: 0.4rem;
    }

    &__btn-1 {
      opacity: 0;
    }

    &__btn-4 {
      position: relative;
    }

    &__menu {
      width: max-content;

      list-style: none;

      background-color: #fff;
      border-radius: 2px;
      box-shadow: 0 1px 1rem 1px rgba(0, 0, 0, 0.1);

      position: absolute;
      z-index: 9999;

      opacity: 0%;
      visibility: hidden;

      transition: all 200ms ease-in;
      &-item {
        font-family: "Roboto";
        font-size: 1.4rem;
        color: var(--color-gray-dark-2);
        text-align: left;

        padding: 0.8rem;
        padding-left: 1rem;
        padding-right: 2.4rem;

        z-index: 9999;

        &:first-child {
          margin-top: 0.4rem;
        }

        &:last-child {
          margin-bottom: 0.4rem;
        }

        &:hover {
          background-color: var(--sidenav-item-hover);
        }
      }
    }

    &__controls {
      opacity: 0;

      display: flex;
      align-items: center;
      gap: 0.5rem;

      transition: opacity 200ms ease-in;
    }

    &:hover &__controls {
      opacity: 100%;
    }

    &:hover &__btn-1 {
      opacity: 100%;
    }

    &__select {
      height: 2.4rem;
      width: 2.4rem;

      border-radius: 50%;
      opacity: 0;

      position: absolute;
      top: -12px;
      left: -12px;
      z-index: 2;

      transition: opacity 200ms ease-in, background-color 200ms ease-in;
    }

    &:hover &__select {
      opacity: 100%;
    }

    &--selected {
      border: 2px solid #000;
    }

    &--selected > &__select {
      background-color: #444;
      opacity: 100%;
    }

    &--selected > &__controls,
    &--selected:hover > &__controls {
      opacity: 0;
    }
  }

  // Note DnD states
  .dragged {
    opacity: 20%;
    transform: translateY(5px);
  }
  .hovered {
    box-shadow: 0 5px 2rem 2px rgba(0, 0, 0, 0.1);
    transform: translateY(-4px);
  }

  // Menus visibility
  .visible {
    opacity: 100%;
    visibility: visible;
  }

  .animation:not(:nth-last-child(1)) {
    border-bottom: 1px solid rgba(0, 0, 0, 0.05);
  }

  .animation:first-child {
    border-top: 1px solid rgba(0, 0, 0, 0.1);
  }

  .animation:last-child {
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  }
</style>
