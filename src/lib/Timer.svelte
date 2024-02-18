<script>
	import { appWindow } from '@tauri-apps/api/window';

	// Import necessary dependencies
	import { onMount, onDestroy } from 'svelte';

	// Define component logic
	export let pomodoroDuration = 10;
	export let breakDuration = 5;
	export let autoPlayPomodoro = false;
	export let autoPlayBreak = false;

	let timeLeft = pomodoroDuration;
	let timer = 0;
	let inBreak = false;
	let inPlay = false;
	// Function to start the timer

	function startTimer() {
		timer = setInterval(() => {
			if (inPlay && timeLeft > 0) {
				timeLeft--;
			} else {
				if (inPlay && timeLeft <= 0) {
					if (inBreak) {
						console.warn('in brak', timeLeft);
						appWindow.toggleMaximize();
						reset();
						if (!autoPlayPomodoro) {
							pauseTimer();
						}
					} else {
						console.warn('in not in break');
						inBreak = true;
						timeLeft = breakDuration;
						appWindow.toggleMaximize();
						//autoplay break
						if (!autoPlayBreak) {
							pauseTimer();
						}
					}
				} //else {
				//@ts-checkrval(timer);
				//}
			}
		}, 1000);
	}

	// Function to pause the timer
	function pauseTimer() {
		inPlay = false;
		clearInterval(timer);
	}

	// Function to resume the timer
	function resumeTimer() {
		inPlay = true;
		startTimer();
	}

	// Function to toggle play/pause
	function playOrPause() {
		if (inPlay) {
			inPlay = false;
			clearInterval(timer);
			timer = 0;
		} else {
			inPlay = true;
			startTimer();
		}
	}

	function reset() {
		timeLeft = pomodoroDuration;
		clearInterval(timer);
		inBreak = false;
		inPlay = false;
	}

	// Lifecycle hook to start the timer when the component is mounted
	//onMount(startTimer);

	// Lifecycle hook to clear the timer when the component is destroyed
	onDestroy(() => clearInterval(timer));
</script>

<!-- Component markup -->
<div>
	{#if inPlay}
		<h2>{inBreak ? 'Well-deserved break' : 'Stay focus'}</h2>
	{:else}
		<h2>{inBreak ? 'Time to take a break' : "Let's focus"}</h2>
	{/if}

	<button on:click={playOrPause}>{inPlay ? 'Pause' : 'Play'}</button>
	<button on:click={() => (timeLeft = 60)}>Reset</button>
	<p>Time Left: {timeLeft}</p>
</div>
