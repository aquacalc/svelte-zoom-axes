<script>
  // D3 in Svelte for WQ Map
  // see: https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte
  // and: https://d3-graph-gallery.com/graph/interactivity_zoom.html
  // ** my mod of latter...
  // /Users/nickstaresinic/Documents/Documents/Udemy Courses/Svelte/html-zoom/html-zoom-axes/script-modified.js
  import { select } from "d3-selection";
  import { scaleLinear } from "d3-scale";
  import { zoom } from "d3-zoom";

  export let dummyData;
  import Axis from "./Axis.svelte";

  // ** -- Set Chart Dimensions -- ** //
  let svgWidth = 460;
  let svgHeight = 400;

  let margin = { top: 40, right: 30, bottom: 40, left: 60 };

  let width = svgWidth - margin.left - margin.right;
  let height = svgHeight - margin.top - margin.bottom;

  // Re-scaled x & y
  let newScaleX;
  let newScaleY;

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

  // let zoomIt = () => {
  //   console.log(`Proba...`);

  // $: {
  //   console.log(`In reactive 'zoom behavior'...`);
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

  //   // updateChart();
  // }

  // function updateChart({ transform }) {
  //   console.log(`UP_DATE Chart...`);
  // }

  // };

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

  function addInvizRect(node) {
    // console.log(`UPDATE_CHART...`, node);
    // console.log(`UPDATE_CHART... select('svg')`, select('svg'));

    // Chain ".call(zoom)" to the 'invisible' <rect> ??
    let invizRect = select("#wq-svg")._groups[0][0].lastChild;
    // console.log(`UPDATE_CHART... select 'invisible' <rect>: `, invizRect);

    select("#wq-svg")
      .attr("id", "invisible-rect")
      .append("rect")
      .attr("pointer-events", "all")
      .attr("transform", `translate(${margin.left}, ${margin.top})`)
      .style("fill", "skyblue")
      .style("opacity", "0.35")
      // .style("fill", "#f5f5dc73")
      // .attr("fill", "none")
      .style("outline", "1px solid blue")
      .attr("width", width)
      .attr("height", height)
      .call(
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
          .on("zoom", zoomed)
      );

    // Implement zoom behavior
    function zoomed({ transform }) {
      console.log(`Zoom Transform`, transform);

      // recover the new scale
      newScaleX = transform.rescaleX(xScale);
      newScaleY = transform.rescaleY(yScale);

        // update axes with these new boundaries
        // xAxis.call(axisBottom(newScaleX));
        // yAxis.call(axisLeft(newScaleY));
    }

    // node.onclick = () => alert("Evo!");
    // node.style.fill = params;
    // node.style.opacity = 0.2;
    return {
      // update(newParams) {
      //   console.log(`Action update with `, newParams);
      // },

      destroy() {
        console.log(`Ex-ter-min-ate!`, node);
      },
    };
    // console.log(`event: `, e);
  }
</script>

<h2>The WQ Map (Four: Add Svelte Action, zoom & pan axes)</h2>
<p>
  <em>see</em>:
  <a href="https://stackoverflow.com/questions/57956581/svelte-and-d3-brush"
    >https://stackoverflow.com/questions/57956581/svelte-and-d3-brush</a
  >
</p>

<div class="scatter-plot-div" bind:clientWidth={svgWidth}>
  {#if svgWidth}
    <svg id="wq-svg" width={svgWidth} height={svgHeight} use:addInvizRect>
      <Axis {width} {height} {margin} scale={newScaleX ? newScaleX : xScale} position="bottom" />
      <Axis {width} {height} {margin} scale={newScaleY ? newScaleY : yScale} position="left" />

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
      <!-- <rect
        id="invisible-rect"
        {width}
        {height}
        pointer-events="all"
        transform={`translate(${margin.left}, ${margin.top})`}
      /> -->
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

  /* #invisible-rect {
    fill: #c8f4a743;
    outline: 2px solid blue;
  } */

  h2 {
    color: rebeccapurple;
    margin-bottom: 0px;
  }

</style>
