# svelte app

Port existing WQ Map (https://www.aquatoolbox.com/scroll-wqmap.html) from its D3-fueled React component to Svelte.

- _see_: https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/ScatterPlot.svelte
- _and_: https://www.youtube.com/watch?v=vHHLLJA0b70&list=PL8bMgX1kyZThM1sbYCoWdTcpiYysJsSeu&index=2&t=4624s
- _and_: https://d3-graph-gallery.com/graph/interactivity_zoom.html
- _and_: https://www.freecodecamp.org/news/get-ready-to-zoom-and-pan-like-a-pro-after-reading-this-in-depth-tutorial-5d963b0a153e/
- ([NB] \*\* my modifications of latter in... html-zoom-axes/script-modified.js)

# WqMap_One

Just recreate and svg with a few data points as circles and D3 x- and y-scales as reactive statements

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
