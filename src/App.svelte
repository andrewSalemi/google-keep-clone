<script>
  import Header from "./components/Header.svelte";
  import SideNav from "./components/SideNav.svelte";
  import Note from "./components/Note.svelte";
  import NoteGenerator from "./components/NoteGenerator.svelte";
</script>

<div class="header-wrapper">
  <Header />
</div>
<div class="container">
  <div class="sidenav-wrapper">
    <SideNav />
  </div>
  <main class="main">
    <div class="main__note-generator">
      <NoteGenerator />
    </div>
    <section class="section__note">
      <Note />
      <Note />
      <Note />
      <Note />
      <Note />
      <Note />
      <Note />
      <Note />
      <Note />
      <Note />
    </section>
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

    display: flex;
    flex-direction: column;
    row-gap: 2.8rem;

    &__note-generator {
      align-self: center;
      grid-column: 1 / -1;
      grid-row: 1 / 2;
    }
  }

  .section__note {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    column-gap: 1.2rem;
    row-gap: 2rem;
  }
</style>
