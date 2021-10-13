<script>
  import Header from "./components/Header.svelte";
  import SideNav from "./components/SideNav.svelte";
  import Note from "./components/Note.svelte";
  import NoteGenerator from "./components/NoteGenerator.svelte";
  import { notes } from "./store/notes";
  import { selectedNotes } from "./store/selectedNotes";
  import { flip } from "svelte/animate";

  let hovered = false;

  // Drag and Drop
  let startDrag = (event, noteIdx) => {
    event.dataTransfer.effectAllowed = "move";
    event.dataTransfer.dropEffect = "move";
    const start = noteIdx;
    event.dataTransfer.setData("text/plain", start); // Storing note idx for next the next event
    console.log("Start drag");
  };

  let handleDrop = (event, noteHoverIdx) => {
    console.log("dropped");
    event.dataTransfer.dropEffect = "move";
    const start = parseInt(event.dataTransfer.getData("text/plain"));

    console.log(`Moving note ${start} over note ${noteHoverIdx}`);
    notes.update((oldNotes) => {
      if (start < noteHoverIdx) {
        oldNotes.splice(noteHoverIdx + 1, 0, oldNotes[start]);
        oldNotes.splice(start, 1);
      } else {
        oldNotes.splice(noteHoverIdx, 0, oldNotes[start]);
        oldNotes.splice(start + 1, 1);
      }
      return oldNotes;
    });

    hovered = -1;
  };

  // Selection
  let handleSelection = (event, idx) => {
    console.log(event.detail.selectionStatus);
    if (!event.detail.selectionStatus) {
      selectedNotes.update((oldSelection) => {
        return oldSelection.filter((selection) => selection !== $notes[idx]);
      });
    } else {
      selectedNotes.update((oldSelection) => {
        return [...oldSelection, $notes[idx]];
      });
    }
    console.log($selectedNotes);
  };
</script>

<div class="header">
  <Header />
</div>

<main class="main">
  <div class="main__sidenav">
    <SideNav />
  </div>
  <section class="main__section">
    <div on:do class="main__generator">
      <NoteGenerator />
    </div>
    <div class="main__notes">
      {#each $notes as note, idx (note)}
        <div
          class="main__note"
          class:hovered={hovered === idx}
          draggable="true"
          on:dragstart={(event) => startDrag(event, idx)}
          on:drop|preventDefault={(event) => handleDrop(event, idx)}
          on:dragenter={() => (hovered = idx)}
          ondragover="return false"
          animate:flip={{ duration: 200 }}
        >
          <!-- ondragover="return false stop the default behaviour" -->
          <Note
            title={note.noteTitle}
            content={note.noteContent}
            on:selection={(event) => handleSelection(event, idx)}
          />
        </div>
      {/each}
    </div>
  </section>
</main>

<style lang="scss">
  .header {
    width: 100vw;
    background-color: #fff;

    border-bottom: 1px solid var(--color-gray-light-2);
    padding: 0.8rem 1.2rem;

    position: fixed;
    top: 0;
    left: 0;
    z-index: 1000;
  }

  .main {
    position: absolute;
    margin-top: 6.4rem;
    width: 100%;
    height: calc(100vh - 6.4rem); // screen h - header h
    display: flex;
    gap: 2.8rem;

    &__sidenav {
      min-width: max-content;
      position: fixed;
    }
    &__section {
      display: flex;
      flex-direction: column;
      gap: 1.2rem;
    }

    &__generator {
      justify-self: center;
      align-self: center;
      margin: 3.2rem auto 1.6rem auto;
    }
    &__notes {
      margin-left: 28rem;
      display: flex;
      flex-wrap: wrap;
      column-gap: 2.4rem;
      row-gap: 1.8rem;
    }

    &__note {
      border-radius: 9px;

      transition: all 200ms ease-in;
    }
  }

  .hovered {
    box-shadow: 0 5px 2rem 2px rgba(0, 0, 0, 0.1);
    transform: translateY(-2px);
  }
</style>
