<script>
  import Btn from "./Btn.svelte";
  import { createEventDispatcher } from "svelte";

  export let content = "";
  export let title = "";

  let selected = false;

  const dispatch = createEventDispatcher();

  let handleSelection = () => {
    selected = !selected;
    dispatch("selection", {
      selectionStatus: selected,
    });
  };
</script>

<article class="note" class:note--selected={selected}>
  <img class="note__select" src="assets/icons/icon-select.svg" alt="Select" on:click={handleSelection} />
  <div class="note__header">
    <h3 class="note__title">{title}</h3>
    <div class="note__btn-1">
      <Btn iconName="pin" btnSmall={true} />
    </div>
  </div>
  <p class="note__content">
    {content}
  </p>
  <p />
  <div class="note__controls">
    <div class="note__btn-2">
      <Btn iconName="bellPlus" btnXSmall={true} />
    </div>
    <div class="note__btn-3">
      <Btn iconName="userPlus" btnXSmall={true} />
    </div>
    <div class="note__btn-4">
      <Btn iconName="palette" btnXSmall={true} />
    </div>
    <div class="note__btn-5">
      <Btn iconName="image" btnXSmall={true} />
    </div>
    <div class="note__btn-6">
      <Btn iconName="archive" btnXSmall={true} />
    </div>
    <div class="note__btn-7">
      <Btn iconName="dots" btnXSmall={true} />
    </div>
  </div>
</article>

<style lang="scss">
  .note {
    align-self: flex-start;

    max-width: 24rem;

    border-radius: 8px;
    border: 1px solid #e0e0e0;
    padding: 1rem 1rem 0.4rem 1rem;
    box-shadow: none;
    background: transparent;

    cursor: pointer;
    border: 2px solid var(--color-gray-light-2);

    position: relative;

    display: flex;
    flex-direction: column;

    transition: box-shadow 200ms ease-in, border 200ms ease-in;

    ::selection {
      background: transparent;
    }
    &:hover {
      box-shadow: 0 1px 2px 0 rgb(60 64 67 / 30%), 0 1px 3px 1px rgb(60 64 67 / 15%);
    }

    &__header {
      display: flex;
      justify-content: space-between;
      align-items: center;

      margin-bottom: 1.2rem;
    }
    &__title {
      font-size: 2rem;
      font-weight: 400;
      color: var(--color-gray-dark-2);

      text-overflow: ellipsis;
      overflow: hidden;
    }

    &__content {
      font-family: "Roboto", sans-serif;
      font-size: 1.6rem;
      font-weight: 400;
      color: var(--color-gray-dark-1);
      word-wrap: break-word;

      margin-bottom: 0.4rem;
    }

    &__btn-1 {
      opacity: 0;
    }

    &__controls {
      opacity: 0;

      display: flex;
      align-items: center;
      gap: 0.5rem;

      transition: opacity 200ms ease-in;
    }

    &:hover &__controls {
      opacity: 100%;
    }

    &:hover &__btn-1 {
      opacity: 100%;
    }

    &__select {
      height: 2.4rem;
      width: 2.4rem;

      border-radius: 50%;
      opacity: 0;

      position: absolute;
      top: -12px;
      left: -12px;
      z-index: 2;

      transition: opacity 200ms ease-in, background-color 200ms ease-in;
    }

    &:hover &__select {
      opacity: 100%;
    }

    &--selected {
      border: 2px solid #000;
    }

    &--selected > &__select {
      background-color: #444;
      opacity: 100%;
    }

    &--selected > &__controls,
    &--selected:hover > &__controls {
      opacity: 0;
    }
  }
</style>
