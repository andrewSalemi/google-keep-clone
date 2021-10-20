<script>
  import Btn from "./Btn.svelte";
  import autosize from "autosize";
  import NoteColorPicker from "./NoteColorPicker.svelte";
  import TodoItem from "./TodoItem.svelte";
  import { createEventDispatcher } from "svelte";
  import { flip } from "svelte/animate";
  import { fade, fly } from "svelte/transition";

  const dispatch = createEventDispatcher();

  export let noteIdx;
  export let noteTitle = "";
  export let noteContent = "";
  export let noteTodos = [];
  export let color = "#fff";

  let itemContent = "";

  let colorPicker = false;
  let todoList;
  let lastCreated;

  const handleChangeColor = (event) => {
    color = event.detail.selectedColor;
  };

  const noteSaveModify = () => {
    dispatch("noteSaveModify", {
      noteIdx: noteIdx,
      newTitle: noteTitle,
      newContent: noteContent,
      newColor: color,
    });
  };

  const createTodoItem = (event) => {
    let newItem = {
      content: itemContent,
      status: false,
    };
    noteTodos.push(newItem);
    noteTodos = noteTodos;
    itemContent = "";
    event.target.blur();
  };

  const handleSaveTodoContent = (event, itemIdx) => {
    let toUpdate = noteTodos[itemIdx];
    toUpdate.content = event.detail.content;
    noteTodos = noteTodos;
    dispatch("updateTodoContent", { itemIdx, content: event.detail.content });
  };

  const handleSaveTodoStatus = (event, itemIdx) => {
    let toUpdate = noteTodos[itemIdx];
    toUpdate.status = event.detail.status;
    noteTodos = noteTodos;
    dispatch("updateTodoStatus", { itemIdx, status: event.detail.status });
  };

  const handleItemDelete = (event, itemIdx) => {
    noteTodos.splice(itemIdx, 1);
    noteTodos = noteTodos;
    if (noteTodos.length === 0) {
      dispatch("deleteTodoNote");
    } else {
      dispatch("todoItemDelete", { noteTodos });
    }
  };
</script>

<section class="note-full" style="background-color: {color}; border-color: {color};">
  <input class="note-full__title" type="text" bind:value={noteTitle} />
  {#if noteTodos.length === 0}
    <textarea
      on:input={(event) => autosize(event.target)}
      class="note-full__content"
      type="text"
      bind:value={noteContent}
    />
  {:else}
    <ul class="note-full__todos" bind:this={todoList}>
      {#each noteTodos as todoItem, idx (todoItem)}
        <div bind:this={lastCreated} class="animation" animate:flip in:fade|local out:fly|local={{ x: 100 }}>
          <TodoItem
            autofocus={true}
            status={todoItem.status}
            content={todoItem.content}
            on:saveTodoContent={(event) => handleSaveTodoContent(event, idx)}
            on:saveTodoStatus={(event) => handleSaveTodoStatus(event, idx)}
            on:deleteItem={(event) => handleItemDelete(event, idx)}
          />
        </div>
      {/each}
      <li class="note-full__todo-menu">
        <span class="note-full__todo-menu-icon">+</span>
        <input
          class="note-full__todo-menu-input"
          type="text"
          placeholder="Voce elenco"
          bind:value={itemContent}
          on:input={createTodoItem}
        />
      </li>
    </ul>
  {/if}
  <div class="note-full__btn-1">
    <Btn iconName="pin" btnXSmall={true} />
  </div>
  <div class="note-full__actions">
    <div class="note-full__btn-2">
      <Btn iconName="bellPlus" btnXSmall={true} />
    </div>
    <div class="note-full__btn-3">
      <Btn iconName="userPlus" btnXSmall={true} />
    </div>
    <div
      class="note-full__btn-4"
      on:pointerenter={() => (colorPicker = true)}
      on:pointerleave={() => (colorPicker = false)}
    >
      <NoteColorPicker {color} isVisible={colorPicker} on:colorChange={(event) => handleChangeColor(event)} />
      <Btn iconName="palette" btnXSmall={true} enabled={true} />
    </div>
    <div class="note-full__btn-5">
      <Btn iconName="image" btnXSmall={true} />
    </div>
    <div class="note-full__btn-6">
      <Btn iconName="archive" btnXSmall={true} />
    </div>
    <div class="note-full__btn-7">
      <Btn iconName="dots" btnXSmall={true} />
    </div>
    <div class="note-full__btn-8">
      <Btn iconName="undo" btnXSmall={true} />
    </div>
    <div class="note-full__btn-9">
      <Btn iconName="undo" btnXSmall={true} />
    </div>
    <button class="note-full__close" on:click={noteSaveModify}>Salva</button>
  </div>
</section>

<style lang="scss">
  .note-full {
    width: 60rem;
    min-height: 20rem;

    border-radius: 8px;
    border: 1px solid var(--color-gray-light-1);
    box-shadow: 1px 2px 0 rgb(60 64 67 / 30%), 0 2px 6px 2px rgb(60 64 67 / 15%);

    display: grid;
    grid-template-columns: 1fr 2.3rem;
    row-gap: 1rem;
    padding-top: 0.4rem;

    transition: all 200ms ease-in;

    &__title {
      grid-column: 1 / 2;
    }

    &__content {
      grid-column: 1 / -1;
    }

    &__todo-menu {
      grid-column: 1 / -1;

      padding: 0.4rem 1rem;
      border-top: 1px solid rgba(0, 0, 0, 0.1);
      border-bottom: 1px solid rgba(0, 0, 0, 0.1);

      display: flex;
      align-items: center;
      gap: 1.2rem;
      &-icon {
        font-family: "Roboto", sans-serif;
        font-size: 2rem;
        line-height: 1.2;
        color: #888;
        margin-left: 2.4rem;
      }

      &-input {
        width: 100%;

        border: transparent;
        background: transparent;
        outline: none;

        &::placeholder {
          font-weight: 700;
        }
      }
    }

    &__title,
    &__content {
      line-height: 1;
      color: var(--color-gray-dark-2);

      border: none;
      background: transparent;
      padding: 1rem 1.8rem;

      outline: none;

      &::placeholder {
        font-weight: 500;
        color: var(--color-gray-dark-1);
      }
    }

    &__title {
      font-size: 1.6rem;
      font-weight: 500;
    }

    &__content {
      font-family: inherit;
      font-size: 1.4rem;

      height: auto;
      max-height: 25rem;
      resize: none;
    }

    &__btn-1 {
      grid-column: 2 / 3;
      grid-row: 1 / 2;

      align-self: center;
      justify-self: center;
      padding-right: 2rem;
    }

    &__btn-4 {
      position: relative;
    }

    &__btn-9 {
      transform: rotateY(180deg);
    }

    &__actions {
      padding: 0.5rem 1rem;

      grid-column: 1 / -1;

      display: flex;
      align-items: center;
      gap: 2rem;
    }

    &__close {
      font-size: 1.4rem;

      background-color: transparent;

      border: none;
      outline: none;
      border-radius: 8px;
      padding: 0.8rem 2.4rem;
      margin-left: auto;
      margin-right: 1.5rem;

      &:hover {
        background-color: rgba(0, 0, 0, 0.05);
      }
    }
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
