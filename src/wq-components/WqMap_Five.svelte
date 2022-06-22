<script>
  // D3 in Svelte for WQ Map
  // ported from React (https://www.aquatoolbox.com/scroll-wqmap.html)
  // see: https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte
  // and: https://d3-graph-gallery.com/graph/interactivity_zoom.html
  // ** my mod of latter...
  // /Users/nickstaresinic/Documents/Documents/Udemy Courses/Svelte/html-zoom/html-zoom-axes/script-modified.js
  import { select } from "d3-selection";
  import { scaleLinear } from "d3-scale";
  import { zoom, zoomIdentity } from "d3-zoom";

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

  // Svelte acion attached to svg
  const addInvizRect = (node) => {
    console.log(`Create addInvizRect...`, node);

    select("#wq-svg")
      .attr("id", "invisible-rect")
      .append("rect")
      .attr("pointer-events", "all")
      .attr("transform", `translate(${margin.left}, ${margin.top})`)
      // .style("fill", "skyblue")
      // .style("opacity", "0.35")
      .style("fill", "#f5f5dc73")
      // .attr("fill", "none")
      .style("outline", "1px solid blue")
      .attr("width", width)
      .attr("height", height)
      // .call(zoom.transform, zoomIdentity.translate(0,0).scale(2.5))
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
    // see @nero: https://stackoverflow.com/questions/16178366/d3-js-set-initial-zoom-level
    // .append("g")
    // .attr('transform', `translate(${0}, ${0})scale(${2.5})`)

    // Implement zoom behavior
    function zoomed({ transform }) {
      // console.log(`Zoom Transform`, transform);

      // Let D3 recalculate the new x- and y-scales
      // [??] Make this $: reactive, like xScale & yScale
      // [??] But...then how get the transform object?
      newScaleX = transform.rescaleX(xScale);
      newScaleY = transform.rescaleY(yScale);

      // update axes with the newly tranform-ed scales
      // by passing newScaleX & newScaleY as
      // (conditional) props to Axis components
    }

    return {
      // update(newParams) {
      //   console.log(`Action update with `, newParams);
      // },

      destroy() {
        console.log(`Ex-ter-min-ate!`, node);
      },
    };
  };

  // Test Svelte action properly destroyed
  let showSvg = true;
</script>

<h2 class="tab-callout"><span>The WQ Map</span></h2>
<h3>Semantic zoom axes & data points</h3>
<p>
  <em>original in React</em>:
  <a
    href="https://www.aquatoolbox.com/scroll-wqmap.html"
    target="_blank"
    rel="noopener noreferrer">https://www.aquatoolbox.com/scroll-wqmap.html</a
  >
</p>
<p>
  <em>see</em>:
  <a
    href="https://svelte.dev/tutorial/actions"
    target="_blank"
    rel="noopener noreferrer">https://svelte.dev/tutorial/actions</a
  >
</p>

<label for="showSvg">
<input type="checkbox" bind:checked={showSvg} name="showSvg" id="showSvg" />
  Show/Hide Svg (confirm addInvizRect's destroy())
</label>

<div class="scatter-plot-div" bind:clientWidth={svgWidth}>
  {#if svgWidth && showSvg}
    <svg id="wq-svg" use:addInvizRect width={svgWidth} height={svgHeight}>
      <Axis
        {height}
        {width}
        {margin}
        scale={newScaleX ? newScaleX : xScale}
        position="bottom"
      />
      <Axis
        {height}
        {width}
        {margin}
        scale={newScaleY ? newScaleY : yScale}
        position="left"
      />
      <Axis
        {height}
        {width}
        {margin}
        scale={newScaleY ? newScaleY : yScale}
        position="right"
      />

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
        id="wq-map-content"
        clip-path="url(#clipPathId)"
        transform={`translate(${margin.left}, ${margin.top})`}
      >
        <!-- <text
        x={xScale(2)}
        y={yScale(3)}
      >
        Yadda: {xScale(2)} ({newScaleX ? newScaleX(2) : xScale(2)})
      </text> -->
        <!-- "test" pH isopleth -->
        <line
          x1={newScaleX ? newScaleX(1.0) : xScale(1.0)}
          y1={newScaleY ? newScaleY(1.0) : yScale(1.0)}
          x2={newScaleX ? newScaleX(4.0) : xScale(4.0)}
          y2={newScaleY ? newScaleY(1.5) : yScale(1.5)}
          stroke="blue"
          stroke-linecap="round"
        />
        <line
          x1={newScaleX ? newScaleX(1.0) : xScale(1.0)}
          y1={newScaleY ? newScaleY(1.0) : yScale(1.0)}
          x2={newScaleX ? newScaleX(2.75) : xScale(2.75)}
          y2={newScaleY ? newScaleY(9.5) : yScale(9.5)}
          stroke="rebeccapurple"
          stroke-width="3"
          stroke-linecap="round"
        />

        {#each dummyData as d}
          <circle
            cx={newScaleX ? newScaleX(d.x) : xScale(d.x)}
            cy={newScaleY ? newScaleY(d.y) : yScale(d.y)}
            r="8"
            fill="#61a3a9"
            opacity="0.5"
          />
        {/each}
      </g>

      <!-- 
    [NB] 'Invisible' rect that captures zoom & pan pointer events
         append-ed to svg within Svelte "addInvizRect" action
   -->
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

  #wq-map-content {
    outline: 1px solid red;
  }

  h2,
  h3 {
    color: rebeccapurple;
    margin: 6px;
  }

  h2 span {
    padding: 4px;
    /* background-color: rgb(229, 241, 252); */
  }

  h2.tab-callout::before {
    content: "";
    border-top: 2px solid #ff3e00;
    border-bottom: 2px solid #ff3e00;
    border-left: 2px solid #ff3e00;
    padding: 4px;
    border-top-left-radius: 6px;
    border-bottom-left-radius: 6px;
    /* background-color: rgb(229, 241, 252); */
    padding-right: 0.5rem;
  }

  h2.tab-callout::after {
    content: "";
    border-top: 2px solid #ff3e00;
    border-bottom: 2px solid #ff3e00;
    border-right: 2px solid #ff3e00;
    padding: 4px;
    border-top-right-radius: 6px;
    border-bottom-right-radius: 6px;
    /* background-color: rgb(229, 241, 252); */
    padding-left: 0.5rem;
  }

  p {
    margin: 8px;
  }
</style>
