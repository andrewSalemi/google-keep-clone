<script>
  import Btn from "./Btn.svelte";
  import TodoItem from "./TodoItem.svelte";
  import autosize from "autosize";
  import NoteColorPicker from "./NoteColorPicker.svelte";
  import { createEventDispatcher } from "svelte";
  import { flip } from "svelte/animate";
  import { fade, fly } from "svelte/transition";
  import Note from "./Note.svelte";

  const dispatch = createEventDispatcher();

  let todoList;
  let lastCreated;
  let titleInput;

  let noteTitle = "";
  let noteContent = "";
  let noteColor = "#fff";

  let noteTodos = [];
  let itemContent = "";

  let closed = true;
  let colorPicker = false;
  let generatorMode = "note";

  const clickOutside = (node) => {
    const handleClick = (event) => {
      if (node && !node.contains(event.target) && !event.defaultPrevented) {
        node.dispatchEvent(new CustomEvent("clickOutside", node));
      }
    };

    document.addEventListener("click", handleClick, true);

    return {
      destroy() {
        document.removeEventListener("click", handleClick, true);
      },
    };
  };

  const generateNote = (event) => {
    if (event.key === "Enter") {
      console.log("generator Mode:" + generatorMode);

      if (noteTitle === "") {
        noteTitle = "Nessun titolo";
      }
      if (noteContent !== "" || noteTodos.length > 0) {
        console.log(noteTitle, noteContent, noteTodos, noteColor);
        dispatch("generateNote", { noteTitle, noteContent, noteTodos, noteColor });
        noteContent = "";
        noteTodos = [];
        event.target.blur();
      }
      noteTitle = "";
      generatorMode = "note";
      closed = true;
    }
  };

  const toggleTodo = () => {
    generatorMode = "todo";
    closed = false;
    titleInput.focus();
  };

  const handleClose = (event) => {
    if (noteTitle === "") {
      noteTitle = "Nessun titolo";
    }
    if (noteContent !== "" || noteTodos.length > 0) {
      dispatch("generateNote", { noteTitle, noteContent, noteTodos, noteColor });
      noteContent = "";
      noteTodos = [];
    }
    noteTitle = "";
    generatorMode = "note";
    closed = true;
  };

  const handleChangeColor = (event) => {
    noteColor = event.detail.selectedColor;
  };

  const handleItemDelete = (event, itemIdx) => {
    noteTodos.splice(itemIdx, 1);
    noteTodos = noteTodos;
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

  const handleSaveTodoContent = (event, todoIdx) => {
    let toUpdate = noteTodos[todoIdx];
    toUpdate.content = event.detail.content;
    noteTodos = noteTodos;
  };

  const handleSaveTodoStatus = (event, todoIdx) => {
    let toUpdate = noteTodos[todoIdx];
    toUpdate.status = event.detail.status;
    noteTodos = noteTodos;
  };
</script>

<section
  class="note-generator"
  class:note-generator--closed={closed === true}
  use:clickOutside
  on:clickOutside={(event) => handleClose(event)}
  on:keydown={generateNote}
  style="background-color: {noteColor}; border-color: {noteColor};"
>
  <input
    class="note-generator__title"
    type="text"
    bind:this={titleInput}
    bind:value={noteTitle}
    placeholder={closed ? "Scrivi una nota..." : "Titolo"}
    on:click={() => (closed = false)}
  />
  {#if !closed}
    {#if generatorMode === "note"}
      <!-- TEXT NOTE MODE -->
      <textarea
        on:input={(event) => autosize(event.target)}
        class="note-generator__content"
        type="text"
        bind:value={noteContent}
        placeholder="Scrivi una nota..."
      />
    {:else}
      <!-- TODO MODE -->
      <ul class="note-generator__todos" bind:this={todoList}>
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
        <li class="note-generator__todo-menu">
          <span class="note-generator__todo-menu-icon">+</span>
          <input
            class="note-generator__todo-menu-input"
            type="text"
            placeholder="Voce elenco"
            bind:value={itemContent}
            on:input={createTodoItem}
          />
        </li>
      </ul>
    {/if}
    <div class="note-generator__btn-1">
      <Btn iconName="pin" btnXSmall={true} />
    </div>
    <div class="note-generator__actions">
      <div class="note-generator__btn-2">
        <Btn iconName="bellPlus" btnXSmall={true} />
      </div>
      <div class="note-generator__btn-3">
        <Btn iconName="userPlus" btnXSmall={true} />
      </div>
      <div
        class="note-generator__btn-4"
        on:pointerenter={() => (colorPicker = true)}
        on:pointerleave={() => (colorPicker = false)}
      >
        <NoteColorPicker {noteColor} isVisible={colorPicker} on:colorChange={(event) => handleChangeColor(event)} />
        <Btn iconName="palette" btnXSmall={true} enabled={true} />
      </div>
      <div class="note-generator__btn-5">
        <Btn iconName="image" btnXSmall={true} />
      </div>
      <div class="note-generator__btn-6">
        <Btn iconName="archive" btnXSmall={true} />
      </div>
      <div class="note-generator__btn-7">
        <Btn iconName="dots" btnXSmall={true} />
      </div>
      <div class="note-generator__btn-8">
        <Btn iconName="undo" btnXSmall={true} />
      </div>
      <div class="note-generator__btn-9">
        <Btn iconName="undo" btnXSmall={true} />
      </div>
      <button class="note-generator__close" on:click={(event) => handleClose(event)}>Chiudi</button>
    </div>
  {:else}
    <div class="note-generator__actions">
      <div class="note-generator__btn-10">
        <Btn iconName="checkBoxed" enabled={true} btnSmall={true} on:click={toggleTodo} />
      </div>
      <div class="note-generator__btn-11">
        <Btn iconName="brush" btnSmall={true} />
      </div>
      <div class="note-generator__btn-12">
        <Btn iconName="image" btnSmall={true} />
      </div>
    </div>
  {/if}
</section>

<style lang="scss">
  .note-generator {
    width: 60rem;

    border-radius: 8px;
    border: 1px solid var(--color-gray-light-1);
    box-shadow: 1px 2px 0 rgb(60 64 67 / 30%), 0 2px 6px 2px rgb(60 64 67 / 15%);

    display: grid;
    grid-template-columns: 1fr 2.3rem;
    row-gap: 1rem;
    padding-top: 0.4rem;

    transition: all 200ms ease-in;

    &--closed {
      padding: 0;
      height: 4.2rem;
    }

    &__title {
      grid-column: 1 / 2;
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

    &__content {
      grid-column: 1 / -1;
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

    &__todos {
      grid-column: 1 / -1;

      border-top: 1px solid rgba(0, 0, 0, 0.1);
      border-bottom: 1px solid rgba(0, 0, 0, 0.1);
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

    &--closed {
      display: flex;
      align-items: center;

      & > .note-generator__title {
        width: 100%;
        margin-right: auto;
      }

      & > .note-generator__actions {
        gap: 0.5rem;
      }
    }
  }

  .animation:not(:nth-last-child(2)) {
    border-bottom: 1px solid rgba(0, 0, 0, 0.05);
  }
</style>
