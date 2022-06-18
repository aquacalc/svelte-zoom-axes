<script>
  // Harness for WQ Map
  import WqMap_One from "./WqMap_One.svelte";
  import WqMap_Two from "./WqMap_Two.svelte";
  import WqMap_Three from "./WqMap_Three.svelte";
  import WqMap_Four from "./WqMap_Four.svelte";
</script>

<h3>Evo me!</h3>
<!-- Basic: just scatter points -->
<!-- <WqMap_One /> -->

<!-- Add axis components -->
<!-- <WqMap_Two /> -->

<!-- Add clip path -->
<!-- <WqMap_Three /> -->

<!-- Semantic Zoom (1st try) -->
<WqMap_Four />

<style>
  h3 {
    color: #ff3e00;
    font-size: 2em;
    font-weight: 100;
  }
</style>
