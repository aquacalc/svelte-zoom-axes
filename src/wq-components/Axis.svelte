<script>
  // see: Tom Février's https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte

  import { select, selectAll } from "d3-selection";
  import { axisBottom, axisLeft, axisRight } from "d3-axis";

  export let width;
  export let height;
  export let margin;
  export let position;
  export let scale;

  let transform;
  let g;

  $: {
    select(g).selectAll("*").remove();

    let axis;

    switch (position) {
      case "bottom":
        axis = axisBottom(scale).tickSizeOuter(0).ticks(7);
        transform = `translate(${margin.left}, ${height + margin.top})`;
        break;

      case "left":
        axis = axisLeft(scale).tickSizeOuter(0).ticks(7);
        transform = `translate(${margin.left}, ${margin.top})`;
        break;

      case "right":
        axis = axisRight(scale).tickSizeOuter(0).ticks(7);
        transform = `translate(${width + margin.left}, ${margin.top})`;
        break;
    }

    select(g).call(axis);
  }
</script>

<g class="axis" bind:this={g} {transform} />
