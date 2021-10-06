<script>
  import Header from "./components/Header.svelte";
  import SideNav from "./components/SideNav.svelte";
  import Note from "./components/Note.svelte";
</script>

<div class="header-wrapper">
  <Header />
</div>
<div class="container">
  <div class="sidenav-wrapper">
    <SideNav />
  </div>
  <main class="main">
    <Note />
  </main>
</div>

<style lang="scss">
  .container {
    display: grid;
    grid-template-columns: max-content 1fr;

    align-content: center;
    column-gap: 2.4rem;
  }
  .header-wrapper {
    padding: 0.8rem 1.2rem;
    border-bottom: 1px solid var(--color-gray-light-2);
  }

  .sidenav-wrapper {
    width: fit-content;
  }

  .main {
    padding-top: 4.8rem;
    justify-self: center;
  }
</style>
