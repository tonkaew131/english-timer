<script lang="ts">
	const MAX_TIME = 5 * 60 * 1000;

	// Number of milliseconds
	let timeA = {
		timeElapsed: MAX_TIME / 2,
		lastTime: Date.now()
	};
	let timeB = {
		timeElapsed: MAX_TIME / 2,
		lastTime: Date.now()
	};
	let currentPlayer: 'A' | 'B' | 'stop' | 'end' = 'stop';

	setInterval(() => {
		// Make a count down
		if (currentPlayer === 'A') {
			timeA.timeElapsed -= Date.now() - timeA.lastTime;
			timeA.lastTime = Date.now();
		} else if (currentPlayer === 'B') {
			timeB.timeElapsed -= Date.now() - timeB.lastTime;
			timeB.lastTime = Date.now();
		} else if (currentPlayer === 'end') {
			timeA.timeElapsed = MAX_TIME / 2;
			timeB.timeElapsed = MAX_TIME / 2;
			currentPlayer = 'stop';
		}

		if (timeA.timeElapsed <= 0) {
			timeA.timeElapsed = 0;
		}
		if (timeB.timeElapsed <= 0) {
			timeB.timeElapsed = 0;
		}
		if (timeA.timeElapsed === 0 && timeB.timeElapsed === 0) {
			currentPlayer = 'end';
		}
	}, 250);

	function formatTime(ms: number) {
		// XX:XX
		const minutes = Math.floor(ms / 60000);
		const seconds = Math.floor((ms % 60000) / 1000);
		return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
	}

	function switchPlayer(mode: 'A' | 'B') {
		if (currentPlayer === 'stop') {
			timeA.lastTime = Date.now();
			timeB.lastTime = Date.now();
			currentPlayer = mode;
		} else if (currentPlayer === 'A') {
			currentPlayer = 'B';
		} else if (currentPlayer === 'B') {
			currentPlayer = 'A';
		}
	}
</script>

<div class="w-screen h-screen flex flex-col p-3 gap-3">
	<button
		class={`btn w-full h-full text-6xl font-bold rotate-180 ${
			currentPlayer === 'A' ? 'variant-soft-success' : 'variant-soft-surface'
		}`}
		on:click={() => switchPlayer('A')}
	>
		{formatTime(timeA.timeElapsed)}
	</button>
	<button
		class={`btn w-full h-full text-6xl font-bold ${
			currentPlayer === 'B' ? 'variant-soft-success' : 'variant-soft-surface'
		}`}
		on:click={() => switchPlayer('B')}
	>
		{formatTime(timeB.timeElapsed)}
	</button>
</div>
