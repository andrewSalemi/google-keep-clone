<script>
  export let iconName = undefined;
  export let btnSmall = false;
  export let btnXSmall = false;
</script>

<button class="btn {btnSmall ? 'btn--small' : ''} {btnXSmall ? 'btn--xsmall' : ''}">
  <img class="btn__icon" src="assets/icons/icon-{iconName}.svg" alt="Button icon" />
</button>

<style lang="scss">
  .btn {
    height: 4.8rem;
    width: 4.8rem;

    border: none;
    border-radius: 50%;
    background-color: transparent;

    cursor: pointer;

    display: flex;
    justify-content: center;
    align-items: center;

    transition: opacity 200ms ease-in;

    &--small {
      height: 4rem;
      width: 4rem;
    }

    &--xsmall {
      height: 3.2rem;
      width: 3.2rem;
    }

    &__icon {
      filter: opacity(60%);

      height: 2.4rem;
      width: 2.4rem;
    }

    &--xsmall &__icon {
      height: 1.8rem;
      width: 1.8rem;
    }

    &:hover {
      background-color: var(--btn-hover-bg);
    }

    &:hover &__icon {
      filter: opacity(100%);
    }

    &--small:hover &__icon {
      filter: opacity(60%);
    }
  }
</style>
