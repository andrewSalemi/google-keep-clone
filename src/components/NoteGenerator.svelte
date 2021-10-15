<script>
  import Btn from "./Btn.svelte";
  import autosize from "autosize";
  import { notes } from "../store/notes";
  import { searchNotes } from "../store/searchNotes";
  import NoteColorPicker from "./NoteColorPicker.svelte";

  let noteTitle = "";
  let noteContent = "";
  let noteColor = "#fff";
  let closed = true;
  let colorPicker = false;

  function clickOutside(element, callbackFunction) {
    function onClick(event) {
      if (!element.contains(event.target)) {
        callbackFunction();
      }
    }

    document.body.addEventListener("click", onClick);

    return {
      update(newCallbackFunction) {
        callbackFunction = newCallbackFunction;
      },
      destroy() {
        document.body.removeEventListener("click", onClick);
      },
    };
  }

  let generateNote = (event) => {
    if (event.key === "Enter") {
      let note = { noteTitle, noteContent, noteColor };
      notes.update((currentNotes) => {
        return [note, ...currentNotes];
      });
      searchNotes.update((currentSearch) => {
        return [note, ...currentSearch];
      });
      noteTitle = "";
      noteContent = "";
      event.target.blur();
      closed = true;
      console.log(event);
    }
  };

  let handleChangeColor = (event) => {
    noteColor = event.detail.selectedColor;
  };
</script>

<section
  class="note-generator"
  class:note-generator--closed={closed === true}
  use:clickOutside={() => (closed = true)}
  on:keydown={generateNote}
  style="background-color: {noteColor}; border-color: {noteColor};"
>
  <input
    class="note-generator__title"
    type="text"
    bind:value={noteTitle}
    placeholder={closed ? "Scrivi una nota..." : "Titolo"}
    on:click={() => {
      closed = false;
    }}
  />
  {#if !closed}
    <textarea
      on:input={(event) => autosize(event.target)}
      class="note-generator__content"
      type="text"
      bind:value={noteContent}
      placeholder="Scrivi una nota..."
    />
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
      <button
        class="note-generator__close"
        on:click={() => {
          closed = true;
        }}>Chiudi</button
      >
    </div>
  {:else}
    <div class="note-generator__actions">
      <div class="note-generator__btn-10">
        <Btn iconName="checkBoxed" btnSmall={true} />
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
</style>
