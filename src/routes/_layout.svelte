<script lang="ts">
  import Nav from "../components/Nav.svelte";

  export let segment: string;
</script>

<Nav segment="{segment}" />

<main id="main-container">
  <slot />
</main>

<style>
  main {
    position: relative;
    text-align: center;
    max-width: 56em;
    background-color: white;
    padding: 2em;
    margin: 0 auto;
    box-sizing: border-box;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
