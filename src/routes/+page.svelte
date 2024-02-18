<script>
	// @ts-nocheck
	import Timer from '../lib/Timer.svelte';
	let pomodoroDuration = 60;
	let breakDuration = 10;
	let autoPlayPomodoro = false;
	let autoPlayBreak = false;
</script>

<div id="container">
	<Timer {pomodoroDuration} {breakDuration} {autoPlayBreak} {autoPlayPomodoro}></Timer>
</div>
