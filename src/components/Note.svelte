<script>
  import Btn from "./Btn.svelte";
</script>

<section class="note">
  <input class="note__title" type="text" placeholder="Titolo" />
  <input class="note__content" type="text" placeholder="Scrivi una nota" />
  <div class="note__btn-1">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>
  <div class="note__btn-2">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>
  <div class="note__btn-3">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>
  <div class="note__btn-4">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>
  <div class="note__btn-5">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>
  <div class="note__btn-6">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>
  <div class="note__btn-7">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>
  <div class="note__btn-8">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>
  <div class="note__btn-9">
    <Btn iconName="lightbulb" btnXSmall={true} />
  </div>

  <span class="note__close">Chiudi</span>
</section>

<style lang="scss">
  .note {
    width: 60rem;

    border: 8px;
    padding: 0.4rem 1rem 1.2rem 1rem;
    box-shadow: 1px 2px 0 rgb(60 64 67 / 30%), 0 2px 6px 2px rgb(60 64 67 / 15%);

    display: grid;
    grid-template-columns: repeat(9, 1fr);
    row-gap: 1.2rem;
    column-gap: 1rem;

    &__title {
      grid-column: 1 / span 8;
    }

    &__content {
      grid-column: 1 / -1;
    }

    &__title,
    &__content {
      font-size: 1.6rem;
      font-weight: 500;
      line-height: 1;
      color: var(--color-gray-dark-2);

      border: none;
      background: transparent;

      outline: none;

      &::placeholder {
        font-family: inherit;
        font-weight: 500;
        color: var(--color-gray-dark-1);
      }
    }

    &__btn-1 {
      grid-column: 9 / 10;
      grid-row: 1 / 2;
    }

    &__close {
    }
  }
</style>
