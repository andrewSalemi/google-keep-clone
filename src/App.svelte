<script>
  import Header from "./components/Header.svelte";
  import SideNav from "./components/SideNav.svelte";
  import Note from "./components/Note.svelte";
  import NoteGenerator from "./components/NoteGenerator.svelte";
  import { notes } from "./store/store";
</script>

<div class="header">
  <Header />
</div>
<div class="sidenav">
  <SideNav />
</div>

<style lang="scss">
  .header {
    width: 100vw;

    border-bottom: 1px solid var(--color-gray-light-2);
    padding: 0.8rem 1.2rem;
  }

  .sidenav {
    height: calc(100% - 64px);
  }
</style>
