# svelte app
Port existing WQ Map in a D3 React component to Svelte
  * _see_: https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte
  * _and_: https://www.youtube.com/watch?v=vHHLLJA0b70&list=PL8bMgX1kyZThM1sbYCoWdTcpiYysJsSeu&index=2&t=4624s
  * _and_: https://d3-graph-gallery.com/graph/interactivity_zoom.html
  * _and_: https://www.freecodecamp.org/news/get-ready-to-zoom-and-pan-like-a-pro-after-reading-this-in-depth-tutorial-5d963b0a153e/
  * ([NB] ** my modifications of latter in... html-zoom-axes/script-modified.js)

# WqMap_One
Just recreate and svg with a few data points as circles and D3 x- and y-scales as reactive statements
  * no axes
  * no zoom-and-pan
  * no semantic rescale()-ing
  * no clip path

# WqMap_Two
Just recreate and svg with a few data points as circles and D3 x- and y-scales as reactive statements
  * add axes -- à la Tom Février (a single axis component) or Matthais Stahl (one component for each axis)
  * no zoom-and-pan
  * no semantic rescale()-ing
  * no clip path
