<script>
	import { select, selectAll } from 'd3-selection';
	import { axisBottom, axisLeft } from 'd3-axis';

	export let width;
	export let height;
	export let margin;
	export let position;
	export let scale;


	let transform;
	let g;

	$: {
		select(g).selectAll('*').remove();

		let axis;

		switch(position) {
			case 'bottom':
				axis = axisBottom(scale).tickSizeOuter(0);
				transform = `translate(${margin.left}, ${height + margin.top})`;
				break;

			case 'left':
				axis = axisLeft(scale).tickSizeOuter(0);
				transform = `translate(${margin.left}, ${margin.top})`;
		}

		select(g).call(axis);
	}
</script>

<g class='axis' bind:this={g} {transform} />