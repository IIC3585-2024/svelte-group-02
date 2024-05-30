<script>
  import Header from "../components/Header.svelte";
  import Body from "../components/Body.svelte";
//   import List from "./components/List.svelte";
</script>

<style>
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 500px;
    background: #009579;
  }

  :global(.container *) {
    font-family: "Quicksand", sans-serif;
  }
</style>

<div class="container">
  <Header />
  <Body />
  <!-- <List /> -->
</div>