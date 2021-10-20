<script>
  let current = "note";
</script>

<nav class="sidenav">
  <ul class="list">
    <li class="list__item" class:list--selected={current === "note"} on:click={() => (current = "note")}>
      <img src="assets/icons/icon-lightbulb.svg" alt="Note" />
      <a href="#Note" class="list__link">Note</a>
    </li>
    <li class="list__item" class:list--selected={current === "promemoria"} on:click={() => (current = "promemoria")}>
      <img src="assets/icons/icon-notification.svg" alt="Note" />
      <a href="#Promemoria" class="list__link">Promemoria</a>
    </li>
    <li class="list__item" class:list--selected={current === "etichette"} on:click={() => (current = "etichette")}>
      <img src="assets/icons/icon-edit.svg" alt="Note" />
      <a href="#Etichette" class="list__link">Modifica etichette</a>
    </li>
    <li class="list__item" class:list--selected={current === "archivio"} on:click={() => (current = "archivio")}>
      <img src="assets/icons/icon-archive.svg" alt="Note" />
      <a href="#Archivio" class="list__link">Archivio</a>
    </li>
    <li class="list__item" class:list--selected={current === "cestino"} on:click={() => (current = "cestino")}>
      <img src="assets/icons/icon-delete.svg" alt="Note" />
      <a href="#Cestino" class="list__link">Cestino</a>
    </li>
  </ul>
  <div class="sidenav__copyright">
    <p>Licenze open-source</p>
  </div>
</nav>

<style lang="scss">
  .sidenav {
    height: 100%;
    width: fit-content;

    padding-top: 0.8rem;

    display: flex;
    flex-direction: column;
    justify-content: space-between;

    &__copyright {
      font-size: 1.4rem;
      font-weight: 400;
      color: var(--color-gray-dark-1);

      margin-left: 2.4rem;
      margin-bottom: 1.4rem;
    }
  }

  .list {
    list-style: none;

    display: flex;
    flex-direction: column;
    justify-content: center;

    &__item {
      padding: 1.2rem;
      padding-right: 7.2rem;
      border-radius: 0 2.5rem 2.5rem 0;

      cursor: pointer;

      display: flex;
      align-items: center;
      gap: 3.2rem;

      img {
        height: 2.4rem;
        width: 2.4rem;
        opacity: 65%;
        margin-left: 1.2rem;
      }

      &:hover {
        background-color: var(--sidenav-item-hover);
      }
    }

    &__link:link,
    &__link:visited {
      font-size: 1.4rem;
      line-height: 1;
      font-weight: 500;
      letter-spacing: 0.75px;
      text-decoration: none;
      color: var(--color-gray-dark-2);
    }

    &--selected {
      background-color: var(--sidenav-item-selected);

      &:hover {
        background-color: var(--sidenav-item-selected);
      }
      img {
        opacity: 85%;
      }
    }
  }
</style>
