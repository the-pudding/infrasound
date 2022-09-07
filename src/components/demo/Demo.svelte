<script>
	// import DemoLink from "$components/demo/Demo.Link.svelte";
	// import DemoImg from "$components/demo/Demo.Img.svelte";
	// import DemoButtonSet from "$components/demo/Demo.ButtonSet.svelte";
	// import DemoRange from "$components/demo/Demo.Range.svelte";
	// import DemoSelect from "$components/demo/Demo.Select.svelte";
	// import DemoToggle from "$components/demo/Demo.Toggle.svelte";
	// import DemoSortTable from "$components/demo/Demo.SortTable.svelte";
	// import DemoLayerCake from "$components/demo/Demo.LayerCake.svelte";
	// import DemoSvelteElement from "$components/demo/Demo.SvelteElement.svelte";
	// import DemoSvelteComponent from "$components/demo/Demo.SvelteComponent.svelte";
	// import DemoScrolly from "$components/demo/Demo.Scrolly.svelte";
	import { onMount } from "svelte";
	import audioFile from "$data/horror.mp3";

	let AudioContext;
	let audioCtx;
	let analyser;
	let audioHTML;
	let source;
	let filter;
	let isPlaying = false;

	onMount(() => {
		audioHTML = document.getElementById('audio-file');
	});

	function setupCtx() {
		AudioContext = window.AudioContext || window.webkitAudioContext;
		audioCtx = new AudioContext();
	}

	function setupSource() {
		if(!source) {
			source = audioCtx.createMediaElementSource(audioHTML);
			analyser = audioCtx.createAnalyser();

			setupFilter();

			source.connect(analyser);
			source.connect(filter)
			filter.connect(audioCtx.destination);
			console.log(source, analyser, audioCtx.destination);
		}
	}

	function setupFilter() {
		filter = audioCtx.createBiquadFilter();
		filter.type = "bandpass"
		filter.frequency.value = 2000;
	}

	// Create web audio api context
	function handleClick() {
		setupCtx();
		setupSource();
		if (isPlaying) {
			audioHTML.pause();
		} else {
			audioHTML.play();
		}
		isPlaying = !isPlaying;
	}
</script>

<div id="demo">
	<h1>Demo</h1>
	<audio id="audio-file">
		<source src={audioFile} type="audio/mpeg">
	</audio>
	<button on:click="{handleClick}">Play sound</button>
	<!-- <DemoLink />
	<DemoImg />
	<DemoButtonSet />
	<DemoRange />
	<DemoSelect />
	<DemoToggle />
	<DemoSortTable />
	<DemoLayerCake />
	<DemoSvelteElement />
	<DemoSvelteComponent />
	<DemoScrolly /> -->
</div>

<style>
	#demo {
		max-width: 40rem;
		padding: 16px;
		margin: 0 auto;
	}

	:global(#demo section) {
		margin: 32px auto;
		padding-top: 32px;
	}

	:global(#demo h2 span) {
		background: var(--color-mark);
		padding: 0 8px;
	}
</style>
