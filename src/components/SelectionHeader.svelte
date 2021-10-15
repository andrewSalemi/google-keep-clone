<script>
  import Btn from "./Btn.svelte";
  import NoteColorPicker from "./NoteColorPicker.svelte";
  import { selectedNotes } from "../store/selectedNotes";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let colorPicker = false;
  let showMenu = false;

  let handleSelectionColorChange = (event) => {
    dispatch("selectionColorChange", { selectedColor: event.detail.selectedColor });
  };

  let handleSelectionDeletion = () => {
    dispatch("selectionDeletion");
  };
</script>

<header class="header-selection">
  <div class="header-selection__btn-close">
    <Btn enabled={true} iconName={"close"} />
  </div>
  <span class="header-selection__text">
    {$selectedNotes.length}&nbsp;selezionat{$selectedNotes.length > 1 ? "i" : "o"}
  </span>
  <div class="header-selection__actions">
    <div class="header-selection__btn-1">
      <Btn iconName={"pin--blue"} iconRef="opacity-1" />
    </div>
    <div class="header-selection__btn-2">
      <Btn iconName={"notification--blue"} iconRef="opacity-1" />
    </div>
    <div
      class="header-selection__btn-3"
      on:pointerenter={() => (colorPicker = true)}
      on:pointerleave={() => (colorPicker = false)}
    >
      <Btn iconName={"palette--blue"} enabled={true} iconRef="opacity-1" />
      <div class="picker-container">
        <NoteColorPicker isVisible={colorPicker} on:colorChange={(event) => handleSelectionColorChange(event)} />
      </div>
    </div>
    <div class="header-selection__btn-4">
      <Btn iconName={"archive--blue"} iconRef="opacity-1" />
    </div>
    <div class="header-selection__btn-5">
      <Btn enabled={true} iconName={"dots--blue"} iconRef="opacity-1" on:click={() => (showMenu = !showMenu)} />
      <ul class="header-selection__menu {showMenu ? 'visible' : ''}">
        <li class="header-selection__menu-item" on:click={() => handleSelectionDeletion()}>
          <span>Elimina nota</span>
        </li>
      </ul>
    </div>
  </div>
</header>

<style lang="scss">
  .header-selection {
    width: 100%;
    height: 100%;

    border-bottom: 1px solid var(--color-gray-light-2);
    padding: 0.8rem 1.2rem;

    display: flex;
    justify-content: space-between;
    align-items: center;

    &__btn-close {
      margin-right: 2.4rem;
    }

    &__text {
      font-family: "Roboto";
      font-size: 2.2rem;
      line-height: 1;
      color: var(--color-gray-dark-3);

      margin-right: auto;

      justify-self: flex-start;
    }

    &__btn-3 {
      position: relative;
    }

    &__btn-5 {
      position: relative;
    }

    &__actions {
      display: flex;
      align-items: center;
      gap: 0.6rem;
    }

    &__menu {
      width: max-content;

      list-style: none;

      background-color: #fff;
      border-radius: 2px;
      box-shadow: 0 1px 1rem 1px rgba(0, 0, 0, 0.1);

      position: absolute;
      right: 0;
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

        cursor: pointer;

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

    :global([iconRef="opacity-1"]) {
      filter: opacity(100%);
    }
  }

  .picker-container {
    position: absolute;
    top: 18rem;
  }

  .visible {
    opacity: 100%;
    visibility: visible;
  }
</style>
