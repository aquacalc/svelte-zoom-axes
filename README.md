# svelte app

Port existing Water Quality Map (https://www.aquatoolbox.com/scroll-wqmap.html) from its React component with D3 wrapped in a useEffect hook to Svelte.

- _see_: https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte
- _and_: https://www.youtube.com/watch?v=vHHLLJA0b70&list=PL8bMgX1kyZThM1sbYCoWdTcpiYysJsSeu&index=2&t=4624s
- _and_: https://d3-graph-gallery.com/graph/interactivity_zoom.html
- ([NB] \*\* my modifications of last in... html-zoom-axes repo's script-modified.js file.)
- _and_: https://www.freecodecamp.org/news/get-ready-to-zoom-and-pan-like-a-pro-after-reading-this-in-depth-tutorial-5d963b0a153e/

# Bottom-line at the top

Current Svelte REPL with semantic zooming: https://svelte.dev/repl/32e9d36a3da940c187b7f79fc1fbac38?version=3.48.0

# WqMap_One

Just recreate the svg with a few Iris data points as circles and D3 x- and y-scales as reactive statements

- no axes
- no clip path
- no 'invisible' rect
- no zoom-and-pan
- no semantic rescale()-ing

# WqMap_Two

- add axes -- à la Tom Février (a single axis component) or Matthais Stahl (one component for each axis)
- no clip path
- no 'invisible' rect
- no zoom-and-pan
- no semantic rescale()-ing

# WqMap_Three

- added axes -- à la Tom Février (a single axis component)
- add clip path
- add 'invisible' rect
- no zoom-and-pan
- no semantic rescale()-ing

# WqMap_Four

- added axes -- à la Tom Février (a single axis component)
- added clip path
- added 'invisible' rect
- -try- to capture zoom transform
- no semantic rescale()-ing
