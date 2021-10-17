<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let noteColor = "#fff";
  export let isVisible = false;

  const handleChangeColor = (selectedColor) => {
    noteColor = selectedColor;
    dispatch("colorChange", { selectedColor });
  };
</script>

<ul class="colors-list {isVisible ? 'visible' : ''}">
  <li
    class="color color-1 {noteColor === '#fff' ? 'color--selected' : ''}"
    title="Standard"
    on:click={() => handleChangeColor("#fff")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-2 {noteColor === '#f28b82' ? 'color--selected' : ''}"
    title="Rosso"
    on:click={() => handleChangeColor("#f28b82")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-3 {noteColor === '#fbbc04' ? 'color--selected' : ''}"
    title="Arancione"
    on:click={() => handleChangeColor("#fbbc04")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-4 {noteColor === '#fff475' ? 'color--selected' : ''}"
    title="Giallo"
    on:click={() => handleChangeColor("#fff475")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-5 {noteColor === '#ccff90' ? 'color--selected' : ''}"
    title="Verde"
    on:click={() => handleChangeColor("#ccff90")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-6 {noteColor === '#a7ffeb' ? 'color--selected' : ''}"
    title="Verde acqua"
    on:click={() => handleChangeColor("#a7ffeb")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-7 {noteColor === '#cbf0f8' ? 'color--selected' : ''}"
    title="Blu"
    on:click={() => handleChangeColor("#cbf0f8")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-8 {noteColor === '#aecbfa' ? 'color--selected' : ''}"
    title="Blu scuro"
    on:click={() => handleChangeColor("#aecbfa")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-9 {noteColor === '#d7aefb' ? 'color--selected' : ''}"
    title="Viola"
    on:click={() => handleChangeColor("#d7aefb")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-10 {noteColor === '#fdcfe8' ? 'color--selected' : ''}"
    title="Rosa"
    on:click={() => handleChangeColor("#fdcfe8")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-11 {noteColor === '#e6c9a8' ? 'color--selected' : ''}"
    title="Marrone"
    on:click={() => handleChangeColor("#e6c9a8")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
  <li
    class="color color-12 {noteColor === '#e8eaed' ? 'color--selected' : ''}"
    title="Grigio"
    on:click={() => handleChangeColor("#e8eaed")}
  >
    <img src="assets/icons/icon-checked.svg" alt="icon" />
  </li>
</ul>

<style lang="scss">
  .colors-list {
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 1px 1rem 1px rgba(0, 0, 0, 0.1);
    padding: 1rem;

    position: absolute;

    z-index: 99999;
    top: -12.4rem;

    list-style: none;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 0.6rem;

    opacity: 0;
    visibility: hidden;

    transition: all 200ms ease-in;
  }

  .color {
    height: 3rem;
    width: 3rem;
    border-radius: 50%;

    display: flex;
    justify-content: center;
    align-items: center;

    img {
      opacity: 0;
      visibility: hidden;

      transition: all 100ms ease-in;
    }

    &.color {
      box-shadow: 0 0 0 1px #fff inset;

      &-1 {
        background-color: #fff;
        border: 2px solid var(--color-gray-light-2);
      }
      &-2 {
        background-color: #f28b82;
      }
      &-3 {
        background-color: #fbbc04;
      }
      &-4 {
        background-color: #fff475;
      }
      &-5 {
        background-color: #ccff90;
      }
      &-6 {
        background-color: #a7ffeb;
      }
      &-7 {
        background-color: #cbf0f8;
      }
      &-8 {
        background-color: #aecbfa;
      }
      &-9 {
        background-color: #d7aefb;
      }
      &-10 {
        background-color: #fdcfe8;
      }
      &-11 {
        background-color: #e6c9a8;
      }
      &-12 {
        background-color: #e8eaed;
      }
    }

    &:hover {
      border: 2px solid #000;
    }
  }

  .color--selected img {
    opacity: 50%;
    visibility: visible;
  }

  // Menus visibility
  .visible {
    opacity: 100%;
    visibility: visible;
  }
</style>
