<script>
  // D3 in Svelte for WQ Map
  // see: https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte
  // and: https://d3-graph-gallery.com/graph/interactivity_zoom.html
  // ** my mod of latter...
  // /Users/nickstaresinic/Documents/Documents/Udemy Courses/Svelte/html-zoom/html-zoom-axes/script-modified.js

  import { scaleLinear } from "d3-scale";
  import dummyData from "./dummyData_Two";
  import Axis from "./Axis.svelte";

  // ** -- Set Chart Dimensions -- ** //
  let svgWidth = 460;
  let svgHeight = 400;

  let margin = { top: 40, right: 30, bottom: 40, left: 60 };

  let width = svgWidth - margin.left - margin.right;
  let height = svgHeight - margin.top - margin.bottom;

  // Add X axis
  // [** NB **] ".clamp(true)" breaks the panning!!
  $: xScale = scaleLinear().domain([0, 8]).range([0, width]);

  // Add Y axis
  $: yScale = scaleLinear().domain([0, 8]).range([height, 0]);
</script>

<h2>The WQ Map (Three: clip path)</h2>

<div class="scatter-plot-div" bind:clientWidth={svgWidth}>
  {#if svgWidth}
    <svg width={svgWidth} height={svgHeight}>
      <Axis {width} {height} {margin} scale={xScale} position="bottom" />
      <Axis {width} {height} {margin} scale={yScale} position="left" />

      <!-- clip path definition -->
      <defs>
        <clipPath id="clipPathId">
          <rect
            id="clipRect"
            x={0}
            y={0}
            width={width - 0}
            height={height - 0}
            fill="red"
          />
        </clipPath>
      </defs>

      <!-- <g> that's home to the clip path (where chart content lives) -->
      <g
        id="my-clip-path"
        clip-path="url(#clipPathId)"
        transform={`translate(${margin.left}, ${margin.top})`}
      >
        {#each dummyData as d}
          <circle
            cx={xScale(d.x)}
            cy={yScale(d.y)}
            r="8"
            fill="#61a3a9"
            opacity="0.5"
          />
        {/each}
      </g>
    </svg>
  {/if}
</div>

<style>
  svg {
    background-color: rgb(247, 247, 237);
  }

  .scatter-plot-div {
    border: 2px solid rebeccapurple;
  }

  #my-clip-path {
    outline: 1px solid red;
  }

  h2 {
    color: rebeccapurple;
  }
</style>
