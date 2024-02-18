<script>
	// @ts-nocheck
	import Timer from '../lib/Timer.svelte';
	let pomodoroDuration = 60;
	let breakDuration = 10;
</script>

<div id="container">
	<Timer {pomodoroDuration} {breakDuration}></Timer>
</div>
