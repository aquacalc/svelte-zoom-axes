<script>
  // D3 in Svelte for WQ Map
  // see: https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte
  // and: https://d3-graph-gallery.com/graph/interactivity_zoom.html
  // ** my mod of latter...
  // /Users/nickstaresinic/Documents/Documents/Udemy Courses/Svelte/html-zoom/html-zoom-axes/script-modified.js

  import { scaleLinear } from "d3-scale";
  import { zoom } from "d3-zoom";

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

  // ** -- ZOOM -- ** //
  // Zoom behavior as reactive function in Svelte? //
  // let zoom = zoom()
  //     .scaleExtent([1, 5])
  //     .extent([
  //       [0, 0],
  //       [width, height],
  //     ])
  //     .translateExtent([
  //       [0, 0],
  //       [width, height],
  //     ])
  //     .on("zoom", updateChart);

  let testDblClick = (e) => {
    console.log(`Proba...`, e);

    zoom()
      .scaleExtent([1, 5])
      .extent([
        [0, 0],
        [width, height],
      ])
      .translateExtent([
        [0, 0],
        [width, height],
      ])
      .on("zoom", updateChart);
  };

  // $: {
  //   console.log(`Evo!!`);

  //   zoom()
  //     .scaleExtent([1, 5])
  //     .extent([
  //       [0, 0],
  //       [width, height],
  //     ])
  //     .translateExtent([
  //       [0, 0],
  //       [width, height],
  //     ])
  //     .on("zoom", updateChart);
  // }

  function updateChart() {
    console.log(`UPDATE_CHART...`);
    console.log(`event: `, e);
  }

  // Implement zoom behavior
  // function updateChart() {
  //   // recover the new scale
  //   var newScaleX = d3.event.transform.rescaleX(xScale);
  //   var newScaleY = d3.event.transform.rescaleY(yScale);

  //   // update axes with these new boundaries
  //   xAxis.call(d3.axisBottom(newScaleX));
  //   yAxis.call(d3.axisLeft(newScaleY));

  //   // update circle position
  //   scatter
  //     .selectAll("circle")
  //     .attr("cx", (d) => newScaleX(d.Sepal_Length))
  //     .attr("cy", (d) => newScaleY(d.Petal_Length));
  // }
</script>

<h2>The WQ Map (Four: semantic zoom)</h2>

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

      <!-- // This add an invisible rect on top of the chart area.
  // This rect can recover pointer events: necessary to understand when the user zoom -->
      <rect
        id="invisible-rect"
        {width}
        {height}
        pointer-events="all"
        transform={`translate(${margin.left}, ${margin.top})`}
        on:dblclick={testDblClick}
      />
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

  #invisible-rect {
    fill: #c8f4a743;
    outline: 2px solid blue;
  }

  h2 {
    color: rebeccapurple;
  }
</style>
