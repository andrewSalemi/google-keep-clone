<script>
</script>

<nav class="sidenav">
  <ul class="list">
    <li class="list__item list--selected">
      <img src="assets/icons/icon-lightbulb.svg" alt="Note" />
      <a href="#" class="list__link">Note</a>
    </li>
    <li class="list__item">
      <img src="assets/icons/icon-notification.svg" alt="Note" />
      <a href="#" class="list__link">Promemoria</a>
    </li>
    <li class="list__item">
      <img src="assets/icons/icon-edit.svg" alt="Note" />
      <a href="#" class="list__link">Modifica etichette</a>
    </li>
    <li class="list__item">
      <img src="assets/icons/icon-archive.svg" alt="Note" />
      <a href="#" class="list__link">Archivio</a>
    </li>
    <li class="list__item">
      <img src="assets/icons/icon-delete.svg" alt="Note" />
      <a href="#" class="list__link">Cestino</a>
    </li>
  </ul>
</nav>

<style lang="scss">
  .sidenav {
    height: 100%;
    width: fit-content;
    padding-top: 0.8rem;
  }

  .list {
    list-style: none;

    display: flex;
    flex-direction: column;

    &__item {
      padding: 1.2rem;
      border-radius: 0 2.5rem 2.5rem 0;

      cursor: pointer;

      display: flex;
      align-items: center;
      gap: 3.6rem;

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
      letter-spacing: 0.85px;
      text-decoration: none;
      color: var(--color-gray-dark-2);

      margin-right: 5.2rem;
    }

    &--selected {
      background-color: var(--sidenav-item-selected);

      img {
        opacity: 85%;
      }
    }
  }
</style>
