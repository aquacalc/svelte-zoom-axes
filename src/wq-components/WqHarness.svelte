<script>
  // Harness for WQ Map

  // dummyData for WqMap_One
  // dummyData_Two for all others
  import dummyData from "./data/dummyData";
  import dummyData_Two from "./data/dummyData_Two";

  // Components that step from basic - semantic zooming
  import WqMap_One from "./WqMap_One.svelte";
  import WqMap_Two from "./WqMap_Two.svelte";
  import WqMap_Three from "./WqMap_Three.svelte";
  import WqMap_Four from "./WqMap_Four.svelte";
  import WqMap_Five from "./WqMap_Five.svelte";
</script>

<!-- <h3>Evo me!</h3> -->
<!-- Basic: just scatter points -->
<!-- <WqMap_One {dummyData} /> -->

<!-- Add axis component -->
<!-- <WqMap_Two dummyData={dummyData_Two} /> -->

<!-- Add clip path -->
<!-- <WqMap_Three dummyData={dummyData_Two} /> -->

<!-- Semantic Zoom (Svelte Action: zoom & pan axes) -->
<!-- <WqMap_Four dummyData={dummyData_Two} /> -->

<!-- Semantic Zoom (Svelte Action: zoom & pan axes *and* data) -->
<WqMap_Five dummyData={dummyData_Two} />

<style>
  h3 {
    color: #ff3e00;
    font-size: 2em;
    font-weight: 100;
  }
</style>
