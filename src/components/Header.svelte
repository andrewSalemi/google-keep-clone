<script>
  import Btn from "./Btn.svelte";
</script>

<header class="header">
  <div class="header__sidenav">
    <Btn class="header__menu" iconName="menu" />
    <div class="header__position">
      <img class="header__logo" src="assets/images/keep-logo.png" alt="Keep logo" />
      <p class="header__text">Keep</p>
    </div>
  </div>

  <div class="header__searchbar">
    <Btn class="btn--search" iconName="search" btnSmall={true} />
    <input type="text" class="header__input" placeholder="Cerca" />
    <Btn class="btn--clear" iconName="close" btnSmall={true} />
  </div>

  <div class="header__utility-btn">
    <Btn class="btn--clear" iconName="refresh" />
    <Btn class="btn--clear" iconName="agenda" />
    <Btn class="btn--clear" iconName="settings" />
  </div>

  <div class="header__user-panel">
    <Btn class="btn--clear" iconName="app" />
    <img class="header__profile" src="assets/images/pro-pic.jpg" alt="User profile" />
  </div>
</header>

<style lang="scss">
  .header {
    height: 4.8rem;
    width: 100%;

    display: flex;
    align-items: center;

    // Navigation
    &__sidenav {
      display: flex;
      align-items: center;
      gap: 0.4rem;

      padding-right: 9.2rem;
    }

    &__position {
      display: flex;
      align-items: center;
      gap: 0.8rem;
    }

    &__logo {
      width: 4rem;
      height: 4rem;

      display: block;
    }

    &__text {
      font-size: 2.2rem;
      font-weight: 400;
      color: var(--color-gray-dark-1);
    }

    // SEARCH
    &__searchbar {
      height: 100%;
      width: 40%;

      background-color: var(--background-dark);
      border-radius: 0.8rem;
      padding: 0.2rem 0.8rem;

      margin-right: auto;

      display: flex;
      align-items: center;
      border: 1px solid transparent;

      transition: background-color 200ms ease-out, box-shadow 200ms ease-out;

      &:focus-within {
        background-color: #fff;
        border-left: 1px solid rgba(0, 0, 0, 0.1);
        border-right: 1px solid rgba(0, 0, 0, 0.1);
        border-bottom: 1px solid rgba(65, 69, 73, 0.2);
        box-shadow: var(--box-shadow-standard);
      }
    }

    &__input {
      height: 100%;
      width: 100%;

      font-family: inherit;
      font-size: 1.8rem;
      font-weight: 400;

      margin-left: 1.8rem;
      margin-right: auto;

      flex: 1;

      outline: none;
      border: none;
      background-color: transparent;

      &::placeholder {
        font-size: 1.8rem;
        font-weight: 400;
      }
    }

    // USER
    &__user-panel {
      display: flex;
      align-items: center;
      gap: 1rem;
    }

    &__utility-btn {
      margin-right: 1.6rem;

      display: flex;
      align-items: center;
    }

    &__profile {
      height: 3.2rem;
      width: 3.2rem;

      border-radius: 50%;

      cursor: pointer;

      display: inline-block;
    }
  }
</style>
