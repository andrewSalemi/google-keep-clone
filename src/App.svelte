<script>
  import Header from "./components/Header.svelte";
  import SideNav from "./components/SideNav.svelte";
  import Note from "./components/Note.svelte";
  import NoteFull from "./components/NoteFull.svelte";
  import NoteGenerator from "./components/NoteGenerator.svelte";
  import SelectionHeader from "./components/SelectionHeader.svelte";
  import { notes } from "./store/notes";
  import { flip } from "svelte/animate";
  import { scale } from "svelte/transition";
  import { cubicIn, cubicOut } from "svelte/easing";
  import { onMount } from "svelte";

  let currentlyDragged = -1;
  let currentlyHovered = -1;
  let currentlyOpen = -1;
  let currentPage = "note";

  let showSidenav = true;

  let filterNotes = [];
  let selNotes = [];
  let deletedNotes = [];

  onMount(() => {
    filterNotes = [...$notes];
  });

  $: notesDraggable = filterNotes.length === $notes.length;

  // Note Generation
  const handleNoteGeneration = (event) => {
    const newNote = {
      noteTitle: event.detail.noteTitle,
      noteContent: event.detail.noteContent,
      noteTodos: event.detail.noteTodos,
      noteColor: event.detail.noteColor,
    };

    notes.update((currentNotes) => {
      currentNotes.unshift(newNote);
      return currentNotes;
    });
    filterNotes = [...$notes];
  };

  // Note modify
  const handleNoteOpen = (openNoteIdx) => {
    currentlyOpen = openNoteIdx;
  };

  const closeNote = (event) => {
    if (event.target.id === "noteFull") {
      currentlyOpen = -1;
    }
  };

  const handleNoteModify = (event) => {
    notes.update((oldNotes) => {
      let toUpdate = oldNotes[currentlyOpen];
      toUpdate.noteTitle = event.detail.newTitle;
      toUpdate.noteContent = event.detail.newContent;
      toUpdate.noteTodos = event.detail.newTodos;
      toUpdate.noteColor = event.detail.newColor;
      return oldNotes;
    });
    currentlyOpen = -1;
    filterNotes = [...$notes];
  };

  const handleChangeColor = (event, noteIdx) => {
    notes.update((oldNotes) => {
      let toUpdate = oldNotes[noteIdx];
      toUpdate.noteColor = event.detail.color;
      return oldNotes;
    });

    filterNotes = [...$notes];
  };

  const handelTodoContentUpdate = (event, noteIdx) => {
    notes.update((oldNotes) => {
      let noteToUpdate = oldNotes[noteIdx];
      let todoItemIdx = event.detail.itemIdx;
      let todoItemNewContent = event.detail.content;
      noteToUpdate.noteTodos[todoItemIdx].content = todoItemNewContent;
      return oldNotes;
    });

    filterNotes = [...$notes];
  };

  const handelTodoStatusUpdate = (event, noteIdx) => {
    notes.update((oldNotes) => {
      let noteToUpdate = oldNotes[noteIdx];
      let todoItemIdx = event.detail.itemIdx;
      let todoItemNewStatus = event.detail.status;
      noteToUpdate.noteTodos[todoItemIdx].status = todoItemNewStatus;
      return oldNotes;
    });

    filterNotes = [...$notes];
  };

  const handleTodoItemDelete = (event, noteIdx) => {
    notes.update((oldNotes) => {
      let noteToUpdate = oldNotes[noteIdx];
      let todoItemNewTodos = event.detail.todos;
      noteToUpdate.noteTodos = todoItemNewTodos;
      return oldNotes;
    });

    filterNotes = [...$notes];
  };

  // Drag and Drop
  const startDrag = (event, noteIdx) => {
    event.dataTransfer.effectAllowed = "move";
    event.dataTransfer.dropEffect = "move";
    const start = noteIdx;
    currentlyDragged = noteIdx;
    event.dataTransfer.setData("text/plain", start); // Storing note idx for the next event
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
    filterNotes = [...$notes];
  };

  // Selection
  const handleSelection = (event, idx) => {
    if (!event.detail.selectionStatus) {
      selNotes = selNotes.filter((selection) => selection !== $notes[idx]);
    } else {
      selNotes = [...selNotes, $notes[idx]];
    }
  };

  const handleSelectionColorChange = (color) => {
    notes.update((oldNotes) => {
      let toUpdate = selNotes.map((selection) => {
        return oldNotes.indexOf(selection);
      });
      toUpdate.forEach((idx) => (oldNotes[idx].noteColor = color));
      return oldNotes;
    });
    filterNotes = [...$notes];
  };

  // Searching
  const heandleSearch = (event) => {
    const searchValue = event.detail.searchValue.toLowerCase();
    if (searchValue.length === 0) {
      filterNotes = [...$notes];
    } else {
      filterNotes = [...$notes];
      filterNotes = filterNotes.filter(
        (note) =>
          note.noteContent.toLowerCase().includes(searchValue) || note.noteTitle.toLowerCase().includes(searchValue)
      );
    }
  };

  // Delete
  const handleDelete = (noteIdx, content) => {
    if (filterNotes.length === $notes.length) {
      notes.update((oldNotes) => {
        let deleted = oldNotes.splice(noteIdx, 1);
        deletedNotes = [...deletedNotes, ...deleted];
        return [...oldNotes];
      });
    } else {
      notes.update((oldNotes) => {
        return oldNotes.filter((note) => note.noteContent !== content);
      });
    }
    filterNotes = [...$notes];
  };

  const handelDeleteTodoNote = (noteIdx) => {
    notes.update((oldNotes) => {
      oldNotes.splice(noteIdx, 1);
      return oldNotes;
    });

    filterNotes = [...$notes];
  };

  const handleSelectionDeletion = () => {
    notes.update((oldNotes) => {
      return oldNotes.filter(
        (note) =>
          !selNotes.some(
            (toDelete) =>
              toDelete.noteTitle === note.noteTitle &&
              toDelete.noteContent === note.noteContent &&
              toDelete.noteColor === note.noteColor
          )
      );
    });

    selNotes = [];

    filterNotes = [...$notes];
  };
</script>

<div class="header">
  {#if selNotes.length === 0}
    <Header on:search={(event) => heandleSearch(event)} bind:sidebarOpen={showSidenav} />
  {:else}
    <SelectionHeader
      selectionCounter={selNotes.length}
      on:selectionDeletion={handleSelectionDeletion}
      on:selectionColorChange={(event) => handleSelectionColorChange(event.detail.selectedColor)}
      on:undoSelection={() => (selNotes = [])}
    />
  {/if}
</div>

{#if showSidenav}
  <div class="sidenav">
    <SideNav bind:current={currentPage} />
  </div>
{/if}

<main class="main" class:main--no-sidenav={!showSidenav}>
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
    {#if currentPage === "note"}
      {#each filterNotes as note, idx (note)}
        <div
          class="main__note"
          class:opened={idx === currentlyOpen}
          animate:flip={{ duration: 200 }}
          in:scale|locale={{ duration: 100, start: 0 }}
          out:scale|locale={{ duration: 100, start: 0 }}
        >
          <Note
            title={note.noteTitle}
            content={note.noteContent}
            todos={note.noteTodos}
            color={note.noteColor}
            dragged={currentlyDragged === idx}
            hovered={currentlyHovered === idx}
            draggable={notesDraggable}
            on:selection={(event) => handleSelection(event, idx)}
            on:dragstart={(event) => startDrag(event, idx)}
            on:drop={(event) => handleDrop(event, idx)}
            on:dragenter={() => (currentlyHovered = idx)}
            on:delete={() => handleDelete(idx, note.noteContent)}
            on:deleteTodoNote={() => handelDeleteTodoNote(idx)}
            on:openNote={() => handleNoteOpen(idx)}
            on:changeColor={(event) => handleChangeColor(event, idx)}
            on:updateTodoContent={(event) => handelTodoContentUpdate(event, idx)}
            on:updateTodoStatus={(event) => handelTodoStatusUpdate(event, idx)}
            on:todoItemDelete={(event) => handleTodoItemDelete(event, idx)}
          />
        </div>
      {/each}
    {:else}
      {#each deletedNotes as note, idx (note)}
        <div
          class="main__note"
          class:opened={idx === currentlyOpen}
          animate:flip={{ duration: 200 }}
          in:scale|locale={{ duration: 100, start: 0 }}
          out:scale|locale={{ duration: 100, start: 0 }}
        >
          <Note
            title={note.noteTitle}
            content={note.noteContent}
            todos={note.noteTodos}
            color={note.noteColor}
            dragged={currentlyDragged === idx}
            hovered={currentlyHovered === idx}
            draggable={notesDraggable}
            on:selection={(event) => handleSelection(event, idx)}
            on:dragstart={(event) => startDrag(event, idx)}
            on:drop={(event) => handleDrop(event, idx)}
            on:dragenter={() => (currentlyHovered = idx)}
            on:delete={() => handleDelete(idx, note.noteContent)}
            on:deleteTodoNote={() => handelDeleteTodoNote(idx)}
            on:openNote={() => handleNoteOpen(idx)}
            on:changeColor={(event) => handleChangeColor(event, idx)}
            on:updateTodoContent={(event) => handelTodoContentUpdate(event, idx)}
            on:updateTodoStatus={(event) => handelTodoStatusUpdate(event, idx)}
            on:todoItemDelete={(event) => handleTodoItemDelete(event, idx)}
          />
        </div>
      {/each}
    {/if}
  </div>
</main>

{#if currentlyOpen !== -1}
  <div
    id="noteFull"
    class="note-full"
    class:note-full--show={currentlyOpen !== -1}
    on:click={(event) => closeNote(event)}
  >
    <div
      class="note-full__container"
      in:scale={{ duration: 200, delay: 120, easing: cubicIn, start: 0, opacity: 0 }}
      out:scale={{ duration: 250, delay: 0, easing: cubicOut }}
    >
      <NoteFull
        noteIdx={currentlyOpen}
        noteTitle={$notes[currentlyOpen].noteTitle}
        noteContent={$notes[currentlyOpen].noteContent}
        noteTodos={$notes[currentlyOpen].noteTodos}
        color={$notes[currentlyOpen].noteColor}
        on:noteSaveModify={(event) => handleNoteModify(event)}
        on:updateTodoContent={(event) => handelTodoContentUpdate(event, currentlyOpen)}
        on:updateTodoStatus={(event) => handelTodoStatusUpdate(event, currentlyOpen)}
        on:todoItemDelete={(event) => handleTodoItemDelete(event, currentlyOpen)}
      />
    </div>
  </div>
{/if}

<style lang="scss">
  .header {
    grid-row: 1 /2;
    grid-column: 1 / -1;

    width: 100vw;
    max-height: 6.4rem;
    background-color: #fff;

    z-index: 1000;
  }

  .sidenav {
    grid-row: 2 /-1;
    grid-column: 1 / 2;

    height: 100%;
    top: 6.4rem;
  }

  .main {
    width: 100%;
    height: 100%;

    position: relative;

    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;

    &--no-sidenav {
      grid-column: 1 / -1;
    }

    &__generator {
      width: 100%;
    }

    &__notes {
      width: 90%;
      align-self: flex-start;

      margin: 1.2rem auto 0 1rem;

      display: flex;
      flex-wrap: wrap;
      column-gap: 2.4rem;
      row-gap: 1.8rem;
    }

    &__note {
      opacity: 1;

      transition: opacity 200ms ease-out;
    }
  }

  .opened {
    opacity: 0;
  }

  .note-full {
    width: 100%;
    height: 100%;

    background-color: transparent;

    position: absolute;

    z-index: 999999;

    display: flex;
    justify-content: center;
    align-items: center;

    transition: background-color 200ms ease-in;

    &--show {
      background-color: rgba(0, 0, 0, 0.3);
    }
  }
</style>
