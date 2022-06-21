<script>
  // D3 in Svelte for WQ Map
  // see: https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte
  // and: https://d3-graph-gallery.com/graph/interactivity_zoom.html
  // ** my mod of latter...
  // /Users/nickstaresinic/Documents/Documents/Udemy Courses/Svelte/html-zoom/html-zoom-axes/script-modified.js

  import { scaleLinear } from "d3-scale";

  export let dummyData;

  // ** -- Set Chart Dimensions -- ** //
  let svgWidth = 460;
  let svgHeight = 400;

  let margin = { top: 10, right: 30, bottom: 30, left: 60 };

  let width = svgWidth - margin.left - margin.right;
  let height = svgHeight - margin.top - margin.bottom;

  // Add X axis
  // [** NB **] ".clamp(true)" breaks the panning!!
  $: xScale = scaleLinear().domain([0, 8]).range([0, width]);

  // Add Y axis
  $: yScale = scaleLinear().domain([0, 8]).range([height, 0]);
</script>

<h2>The WQ Map (One)</h2>

<div class="scatter-plot" bind:clientWidth={width}>
  {#if width}
    <svg {width} {height}>
      {#each dummyData as d}
        <circle
          cx={xScale(d.Sepal_Length)}
          cy={yScale(d.Petal_Length)}
          r="8"
          fill="#61a3a9"
          opacity="0.5"
        />
      {/each}
    </svg>
  {/if}
</div>

<style>
  .scatter-plot {
    border: 1px solid rebeccapurple;
  }
  svg {
    background-color: rgb(247, 247, 237);
  }
  h2 {
    color: rebeccapurple;
  }
</style>
