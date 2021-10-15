<script>
  import Header from "./components/Header.svelte";
  import SideNav from "./components/SideNav.svelte";
  import Note from "./components/Note.svelte";
  import NoteGenerator from "./components/NoteGenerator.svelte";
  import { notes } from "./store/notes";
  import { selectedNotes } from "./store/selectedNotes";
  import { searchNotes } from "./store/searchNotes";
  import { flip } from "svelte/animate";
  import { onMount } from "svelte";

  let currentlyDragged = -1;
  let currentlyHovered = -1;

  onMount(() => {
    searchNotes.update((oldSearch) => {
      return [...$notes];
    });
  });

  // Drag and Drop
  let startDrag = (event, noteIdx) => {
    event.dataTransfer.effectAllowed = "move";
    event.dataTransfer.dropEffect = "move";
    const start = noteIdx;
    currentlyDragged = noteIdx;
    event.dataTransfer.setData("text/plain", start); // Storing note idx for next the next event
    let dragImage = new Image();
    event.dataTransfer.setDragImage(dragImage, 40, 40);
    console.log("Start drag");
  };

  let handleDrop = (event, noteHoverIdx) => {
    currentlyDragged = -1;
    currentlyHovered = -1;
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
    searchNotes.update((oldNotes) => {
      return [...$notes];
    });
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

  // Searching
  let heandleSearch = (event) => {
    const searchValue = event.detail.searchValue.toLowerCase();
    searchNotes.update((oldNotes) => {
      if (searchValue.length === 0) {
        return [...$notes];
      } else {
        return oldNotes.filter(
          (note) =>
            note.noteContent.toLowerCase().includes(searchValue) || note.noteTitle.toLowerCase().includes(searchValue)
        );
      }
    });
  };

  // Delete
  let handleDelete = (noteIdx, content) => {
    if ($searchNotes.length === $notes.length) {
      notes.update((oldNotes) => {
        oldNotes.splice(noteIdx, 1);
        return [...oldNotes];
      });
      searchNotes.update((oldNotes) => {
        return [...$notes];
      });
    } else {
      notes.update((oldNotes) => {
        return oldNotes.filter((note) => note.noteContent !== content);
      });
      searchNotes.update((oldNotes) => {
        return [...$notes];
      });
    }
  };
</script>

<div class="header"><Header on:search={(event) => heandleSearch(event)} /></div>

<div class="sidenav"><SideNav /></div>

<main class="main">
  <div class="main__generator">
    <NoteGenerator />
  </div>
  <div
    class="main__notes"
    on:dragend={() => {
      currentlyDragged = -1;
      currentlyHovered = -1;
    }}
  >
    {#each $searchNotes as note, idx (note)}
      <div class="main__note" animate:flip={{ duration: 200 }}>
        <!-- ondragover="return false stop the default behaviour" -->
        <Note
          title={note.noteTitle}
          content={note.noteContent}
          noteColor={note.noteColor}
          dragged={currentlyDragged === idx}
          hovered={currentlyHovered === idx}
          on:selection={(event) => handleSelection(event, idx)}
          on:dragstart={(event) => {
            startDrag(event, idx);
          }}
          on:drop={(event) => handleDrop(event, idx)}
          on:dragenter={() => (currentlyHovered = idx)}
          on:delete={() => handleDelete(idx, note.noteContent)}
        />
      </div>
    {/each}
  </div>
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

  .sidenav {
    position: fixed;
    height: calc(100% - 6.4rem);
    top: 6.4rem;
  }

  .main {
    width: 100%;
    height: calc(100% - 6.4rem);

    margin: 6.4rem auto 0 auto;

    display: flex;
    flex-direction: column;
    justify-content: center;

    &__generator {
      margin: 3.2rem auto 1.6rem auto;
    }

    &__notes {
      align-self: center;

      margin: 2.8rem 2.8rem 0 28rem;

      display: flex;
      flex-wrap: wrap;
      column-gap: 2.4rem;
      row-gap: 1.8rem;
    }
  }
</style>
