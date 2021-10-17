<script>
  import Header from "./components/Header.svelte";
  import SideNav from "./components/SideNav.svelte";
  import Note from "./components/Note.svelte";
  import NoteFull from "./components/NoteFull.svelte";
  import NoteGenerator from "./components/NoteGenerator.svelte";
  import SelectionHeader from "./components/SelectionHeader.svelte";
  import { notes } from "./store/notes";
  import { selectedNotes } from "./store/selectedNotes";
  import { searchNotes } from "./store/searchNotes";
  import { flip } from "svelte/animate";
  import { onMount } from "svelte";

  let currentlyDragged = -1;
  let currentlyHovered = -1;
  let currentlyOpen = -1;

  onMount(() => {
    searchNotes.update((oldSearch) => {
      return [...$notes];
    });
  });

  $: notesDraggable = $searchNotes.length === $notes.length;

  // Note Generation
  const handleNoteGeneration = (event) => {
    const newNote = {
      noteTitle: event.detail.noteTitle,
      noteContent: event.detail.noteContent,
      noteColor: event.detail.noteColor,
    };
    console.log(event.detail);
    notes.update((currentNotes) => {
      return [newNote, ...currentNotes];
    });
    searchNotes.update((currentSearch) => {
      return [newNote, ...currentSearch];
    });
  };

  // Note modify
  const handleNoteOpen = (openNoteIdx) => {
    currentlyOpen = openNoteIdx;
    console.log(currentlyOpen);
  };

  const closeNote = (event) => {
    if (event.target.tagName === "DIV") {
      currentlyOpen = -1;
    }
  };

  // Drag and Drop
  const startDrag = (event, noteIdx) => {
    event.dataTransfer.effectAllowed = "move";
    event.dataTransfer.dropEffect = "move";
    const start = noteIdx;
    currentlyDragged = noteIdx;
    event.dataTransfer.setData("text/plain", start); // Storing note idx for next the next event
    let dragImage = new Image();
    event.dataTransfer.setDragImage(dragImage, 40, 40);
    console.log("Start drag");
  };

  const handleDrop = (event, noteHoverIdx) => {
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
  const handleSelection = (event, idx) => {
    if (!event.detail.selectionStatus) {
      selectedNotes.update((oldSelection) => {
        return oldSelection.filter((selection) => selection !== $notes[idx]);
      });
    } else {
      selectedNotes.update((oldSelection) => {
        return [...oldSelection, $notes[idx]];
      });
    }
  };

  const handleSelectionColorChange = (color) => {
    notes.update((oldNotes) => {
      let toUpdate = $selectedNotes.map((selection) => {
        return oldNotes.indexOf(selection);
      });
      toUpdate.forEach((idx) => (oldNotes[idx].noteColor = color));
      return oldNotes;
    });
    searchNotes.update((oldNotes) => {
      return [...$notes];
    });
  };

  // Searching
  const heandleSearch = (event) => {
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
  const handleDelete = (noteIdx, content) => {
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

  const handleSelectionDeletion = () => {
    notes.update((oldNotes) => {
      return oldNotes.filter(
        (note) =>
          !$selectedNotes.some(
            (toDelete) =>
              toDelete.noteTitle === note.noteTitle &&
              toDelete.noteContent === note.noteContent &&
              toDelete.noteColor === note.noteColor
          )
      );
    });

    selectedNotes.update(() => {
      return [];
    });

    searchNotes.update((oldNotes) => {
      return [...$notes];
    });
  };
</script>

<div class="header">
  {#if $selectedNotes.length === 0}
    <Header on:search={(event) => heandleSearch(event)} />
  {:else}
    <SelectionHeader
      on:selectionDeletion={handleSelectionDeletion}
      on:selectionColorChange={(event) => handleSelectionColorChange(event.detail.selectedColor)}
      on:undoSelection={() => {
        selectedNotes.update((oldNotes) => {
          return [];
        });
      }}
    />
  {/if}
</div>

<div class="sidenav"><SideNav /></div>

<main class="main">
  <div class="main__generator">
    <NoteGenerator on:generateNote={handleNoteGeneration} />
  </div>
  <div
    class="main__notes"
    on:dragend={() => {
      currentlyDragged = -1;
      currentlyHovered = -1;
    }}
  >
    {#each $searchNotes as note, idx (note)}
      <div class="main__note" class:opened={idx === currentlyOpen} animate:flip={{ duration: 200 }}>
        <!-- ondragover="return false stop the default behaviour" -->
        <Note
          title={note.noteTitle}
          content={note.noteContent}
          noteColor={note.noteColor}
          dragged={currentlyDragged === idx}
          hovered={currentlyHovered === idx}
          draggable={notesDraggable}
          on:selection={(event) => handleSelection(event, idx)}
          on:dragstart={(event) => {
            startDrag(event, idx);
          }}
          on:drop={(event) => handleDrop(event, idx)}
          on:dragenter={() => (currentlyHovered = idx)}
          on:delete={() => handleDelete(idx, note.noteContent)}
          on:openNote={() => handleNoteOpen(idx)}
        />
      </div>
    {/each}
  </div>
</main>

{#if currentlyOpen !== -1}
  <div class="note-full" class:note-full--show={currentlyOpen !== -1} on:click={(event) => closeNote(event)}>
    <NoteFull
      noteTitle={$notes[currentlyOpen].noteTitle}
      noteContent={$notes[currentlyOpen].noteContent}
      noteColor={$notes[currentlyOpen].noteColor}
    />
  </div>
{/if}

<style lang="scss">
  .header {
    max-height: 4.8rem;
    width: 100vw;
    background-color: #fff;

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
    height: 100%;

    margin: 6.4rem auto 0 auto;

    position: relative;

    display: flex;
    flex-direction: column;
    justify-content: center;

    &__generator {
      margin: 3.2rem auto 1.6rem auto;
    }

    &__notes {
      align-self: center;

      margin: 2.8rem 2.8rem 0 30rem;

      display: flex;
      flex-wrap: wrap;
      column-gap: 2.4rem;
      row-gap: 1.8rem;
    }
  }

  .opened {
    position: absolute;
    top: 30rem;
    left: 50%;

    transform: translate(-50%, -50%);
  }

  .note-full {
    width: 100%;
    height: 120vh;

    background-color: rgba(0, 0, 0, 0.3);

    position: absolute;
    top: 40%;
    left: 50%;

    z-index: 999999;

    display: flex;
    justify-content: center;
    align-items: center;

    transform: translate(-50%, -50%);
  }
</style>
