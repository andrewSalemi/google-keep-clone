<script>
  import Header from "./components/Header.svelte";
  import SideNav from "./components/SideNav.svelte";
  import Note from "./components/Note.svelte";
  import NoteGenerator from "./components/NoteGenerator.svelte";
  import { notes } from "./store/store";

  let hovered = false;

  let startDrag = (event, noteIdx) => {
    event.dataTransfer.effectAllowed = "move";
    event.dataTransfer.dropEffect = "move";
    const start = noteIdx;
    event.dataTransfer.setData("text/plain", start);
    console.log("Start drag");
  };

  let handleDrop = (event, noteHoverIdx) => {
    console.log("dropped");
    event.dataTransfer.dropEffect = "move";
    const start = parseInt(event.dataTransfer.getData("text/plain"));
    const notesMoved = $notes;

    console.log(`Moving note ${start} over note ${noteHoverIdx}`);

    if (start < noteHoverIdx) {
      notesMoved.splice(noteHoverIdx + 1, 0, notesMoved[start]);
      notesMoved.splice(start, 1);
    } else {
      notesMoved.splice(noteHoverIdx, 0, notesMoved[start]);
      notesMoved.splice(start + 1, 1);
    }

    notes.update((oldNotes) => {
      return [...notesMoved];
    });

    hovered = -1;
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
        >
          <Note title={note.noteTitle} content={note.noteContent + " " + idx} />
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
      gap: 1.2rem;
    }

    &__note {
      transition: all 200ms ease-in;
    }
  }

  .hovered {
    border-radius: 9px;
    box-shadow: 0 5px 2rem 2px rgba(0, 0, 0, 0.1);
    transform: translateY(-2px);
  }
</style>
