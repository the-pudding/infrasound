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
	<div class="glitch">
		<div class="glitch__img"></div>
		<div class="glitch__img"></div>
		<div class="glitch__img"></div>
		<div class="glitch__img"></div>
		<div class="glitch__img"></div>
	</div>
	<h1>Demo</h1>
	<audio id="audio-file">
		<source src={audioFile} type="audio/mpeg">
	</audio>
	<button on:click="{handleClick}">Play sound</button>
</div>

<style>
	#demo {
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

	.glitch {
		position: relative;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
	}

	.glitch__img {
		position: absolute;
		top: calc(-1 * 5px);
		left: calc(-1 * 10px);
		width: calc(100% + 10px * 2);
		height: calc(100% + 5px * 2);
		background: url("/assets/images/treehouse.jpg") no-repeat 50% 0;
		background-color: transparent;
		background-size: cover;
		transform: translate3d(0,0,0);
		background-blend-mode: none;
	}	

	.glitch__img:nth-child(n+2) {
		opacity: 0;
	}

	.glitch__img:nth-child(n+2) {
		animation-duration: 4s;
		animation-delay: 2s;
		animation-timing-function: linear;
		animation-iteration-count: 3;
	}

	.glitch__img:nth-child(2) {
		background-color: transparent;
		background-blend-mode: none;
		animation-name: glitch-anim-1;
	}

	.glitch__img:nth-child(3) {
		background-color: transparent;
		background-blend-mode: none;
		animation-name: glitch-anim-2;
	}

	.glitch__img:nth-child(4) {
		background-color: transparent;
		background-blend-mode: none;
		animation-name: glitch-anim-3;
	}

	.glitch__img:nth-child(5) {
		background-color: #af4949;
		background-blend-mode: overlay;
		animation-name: glitch-anim-flash;
	}

	/* Animations */
	@keyframes glitch-anim-1 {
		0% { 
			opacity: 1;
			transform: translate3d(10px,0,0);
			-webkit-clip-path: polygon(0 2%, 100% 2%, 100% 5%, 0 5%);
			clip-path: polygon(0 2%, 100% 2%, 100% 5%, 0 5%);
		}
		2% {
			-webkit-clip-path: polygon(0 15%, 100% 15%, 100% 15%, 0 15%);
			clip-path: polygon(0 15%, 100% 15%, 100% 15%, 0 15%);
		}
		4% {
			-webkit-clip-path: polygon(0 10%, 100% 10%, 100% 20%, 0 20%);
			clip-path: polygon(0 10%, 100% 10%, 100% 20%, 0 20%);
		}
		6% {
			-webkit-clip-path: polygon(0 1%, 100% 1%, 100% 2%, 0 2%);
			clip-path: polygon(0 1%, 100% 1%, 100% 2%, 0 2%);
		}
		8% {
			-webkit-clip-path: polygon(0 33%, 100% 33%, 100% 33%, 0 33%);
			clip-path: polygon(0 33%, 100% 33%, 100% 33%, 0 33%);
		}
		10% {
			-webkit-clip-path: polygon(0 44%, 100% 44%, 100% 44%, 0 44%);
			clip-path: polygon(0 44%, 100% 44%, 100% 44%, 0 44%);
		}
		12% {
			-webkit-clip-path: polygon(0 50%, 100% 50%, 100% 20%, 0 20%);
			clip-path: polygon(0 50%, 100% 50%, 100% 20%, 0 20%);
		}
		14% {
			-webkit-clip-path: polygon(0 70%, 100% 70%, 100% 70%, 0 70%);
			clip-path: polygon(0 70%, 100% 70%, 100% 70%, 0 70%);
		}
		16% {
			-webkit-clip-path: polygon(0 80%, 100% 80%, 100% 80%, 0 80%);
			clip-path: polygon(0 80%, 100% 80%, 100% 80%, 0 80%);
		}
		18% {
			-webkit-clip-path: polygon(0 50%, 100% 50%, 100% 55%, 0 55%);
			clip-path: polygon(0 50%, 100% 50%, 100% 55%, 0 55%);
		}
		20% {
			-webkit-clip-path: polygon(0 70%, 100% 70%, 100% 80%, 0 80%);
			clip-path: polygon(0 70%, 100% 70%, 100% 80%, 0 80%);
		}
		21.9% {
			opacity: 1;
			transform: translate3d(10px,0,0);
		}
		22%, 100% {
			opacity: 0;
			transform: translate3d(0,0,0);
			-webkit-clip-path: polygon(0 0, 0 0, 0 0, 0 0);
			clip-path: polygon(0 0, 0 0, 0 0, 0 0);
		}
	}

	@keyframes glitch-anim-2 {
		0% { 
			opacity: 1;
			transform: translate3d(calc(-1 * 10px),0,0);
			-webkit-clip-path: polygon(0 25%, 100% 25%, 100% 30%, 0 30%);
			clip-path: polygon(0 25%, 100% 25%, 100% 30%, 0 30%);
		}
		3% {
			-webkit-clip-path: polygon(0 3%, 100% 3%, 100% 3%, 0 3%);
			clip-path: polygon(0 3%, 100% 3%, 100% 3%, 0 3%);
		}
		5% {
			-webkit-clip-path: polygon(0 5%, 100% 5%, 100% 20%, 0 20%);
			clip-path: polygon(0 5%, 100% 5%, 100% 20%, 0 20%);
		}
		7% {
			-webkit-clip-path: polygon(0 20%, 100% 20%, 100% 20%, 0 20%);
			clip-path: polygon(0 20%, 100% 20%, 100% 20%, 0 20%);
		}
		9% {
			-webkit-clip-path: polygon(0 40%, 100% 40%, 100% 40%, 0 40%);
			clip-path: polygon(0 40%, 100% 40%, 100% 40%, 0 40%);
		}
		11% {
			-webkit-clip-path: polygon(0 52%, 100% 52%, 100% 59%, 0 59%);
			clip-path: polygon(0 52%, 100% 52%, 100% 59%, 0 59%);
		}
		13% {
			-webkit-clip-path: polygon(0 60%, 100% 60%, 100% 60%, 0 60%);
			clip-path: polygon(0 60%, 100% 60%, 100% 60%, 0 60%);
		}
		15% {
			-webkit-clip-path: polygon(0 75%, 100% 75%, 100% 75%, 0 75%);
			clip-path: polygon(0 75%, 100% 75%, 100% 75%, 0 75%);
		}
		17% {
			-webkit-clip-path: polygon(0 65%, 100% 65%, 100% 40%, 0 40%);
			clip-path: polygon(0 65%, 100% 65%, 100% 40%, 0 40%);
		}
		19% {
			-webkit-clip-path: polygon(0 45%, 100% 45%, 100% 50%, 0 50%);
			clip-path: polygon(0 45%, 100% 45%, 100% 50%, 0 50%);
		}
		20% {
			-webkit-clip-path: polygon(0 14%, 100% 14%, 100% 33%, 0 33%);
			clip-path: polygon(0 14%, 100% 14%, 100% 33%, 0 33%);
		}
		21.9% {
			opacity: 1;
			transform: translate3d(calc(-1 * 10px),0,0);
		}
		22%, 100% {
			opacity: 0;
			transform: translate3d(0,0,0);
			-webkit-clip-path: polygon(0 0, 0 0, 0 0, 0 0);
			clip-path: polygon(0 0, 0 0, 0 0, 0 0);
		}
	}

	@keyframes glitch-anim-3 {
		0% { 
			opacity: 1;
			transform: translate3d(0, calc(-1 * 5px), 0) scale3d(-1,-1,1);
			-webkit-clip-path: polygon(0 1%, 100% 1%, 100% 3%, 0 3%);
			clip-path: polygon(0 1%, 100% 1%, 100% 3%, 0 3%);
		}
		1.5% {
			-webkit-clip-path: polygon(0 10%, 100% 10%, 100% 9%, 0 9%);
			clip-path: polygon(0 10%, 100% 10%, 100% 9%, 0 9%);
		}
		2% {
			-webkit-clip-path: polygon(0 5%, 100% 5%, 100% 6%, 0 6%);
			clip-path: polygon(0 5%, 100% 5%, 100% 6%, 0 6%);
		}
		2.5% {
			-webkit-clip-path: polygon(0 20%, 100% 20%, 100% 20%, 0 20%);
			clip-path: polygon(0 20%, 100% 20%, 100% 20%, 0 20%);
		}
		3% {
			-webkit-clip-path: polygon(0 10%, 100% 10%, 100% 10%, 0 10%);
			clip-path: polygon(0 10%, 100% 10%, 100% 10%, 0 10%);
		}
		5% {
			-webkit-clip-path: polygon(0 30%, 100% 30%, 100% 25%, 0 25%);
			clip-path: polygon(0 30%, 100% 30%, 100% 25%, 0 25%);
		}
		5.5% {
			-webkit-clip-path: polygon(0 15%, 100% 15%, 100% 16%, 0 16%);
			clip-path: polygon(0 15%, 100% 15%, 100% 16%, 0 16%);
		}
		7% {
			-webkit-clip-path: polygon(0 40%, 100% 40%, 100% 39%, 0 39%);
			clip-path: polygon(0 40%, 100% 40%, 100% 39%, 0 39%);
		}
		8% {
			-webkit-clip-path: polygon(0 20%, 100% 20%, 100% 21%, 0 21%);
			clip-path: polygon(0 20%, 100% 20%, 100% 21%, 0 21%);
		}
		9% {
			-webkit-clip-path: polygon(0 60%, 100% 60%, 100% 55%, 0 55%);
			clip-path: polygon(0 60%, 100% 60%, 100% 55%, 0 55%);
		}
		10.5% {
			-webkit-clip-path: polygon(0 30%, 100% 30%, 100% 31%, 0 31%);
			clip-path: polygon(0 30%, 100% 30%, 100% 31%, 0 31%);
		}
		11% {
			-webkit-clip-path: polygon(0 70%, 100% 70%, 100% 69%, 0 69%);
			clip-path: polygon(0 70%, 100% 70%, 100% 69%, 0 69%);
		}
		13% {
			-webkit-clip-path: polygon(0 40%, 100% 40%, 100% 41%, 0 41%);
			clip-path: polygon(0 40%, 100% 40%, 100% 41%, 0 41%);
		}
		14% {
			-webkit-clip-path: polygon(0 80%, 100% 80%, 100% 75%, 0 75%);
			clip-path: polygon(0 80%, 100% 80%, 100% 75%, 0 75%);
		}
		14.5% {
			-webkit-clip-path: polygon(0 50%, 100% 50%, 100% 51%, 0 51%);
			clip-path: polygon(0 50%, 100% 50%, 100% 51%, 0 51%);
		}
		15% {
			-webkit-clip-path: polygon(0 90%, 100% 90%, 100% 90%, 0 90%);
			clip-path: polygon(0 90%, 100% 90%, 100% 90%, 0 90%);
		}
		16% {
			-webkit-clip-path: polygon(0 60%, 100% 60%, 100% 60%, 0 60%);
			clip-path: polygon(0 60%, 100% 60%, 100% 60%, 0 60%);
		}
		18% {
			-webkit-clip-path: polygon(0 100%, 100% 100%, 100% 99%, 0 99%);
			clip-path: polygon(0 100%, 100% 100%, 100% 99%, 0 99%);
		}
		20% {
			-webkit-clip-path: polygon(0 70%, 100% 70%, 100% 71%, 0 71%);
			clip-path: polygon(0 70%, 100% 70%, 100% 71%, 0 71%);
		}
		21.9% {
			opacity: 1;
			transform: translate3d(0, calc(-1 * 5px), 0) scale3d(-1,-1,1);
		}
		22%, 100% {
			opacity: 0;
			transform: translate3d(0,0,0);
			-webkit-clip-path: polygon(0 0, 0 0, 0 0, 0 0);
			clip-path: polygon(0 0, 0 0, 0 0, 0 0);
		}
	}

	/* Flash */
	@keyframes glitch-anim-flash {
		0%, 5% { 
			opacity: 0.2; 
			transform: translate3d(10px, 5px, 0);
		}
		5.5%, 100% {
			opacity: 0;
			transform: translate3d(0, 0, 0);
		}
	}
</style>
