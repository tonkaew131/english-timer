<script lang="ts">
	import { ProgressBar } from '@skeletonlabs/skeleton';

	const MAX_TIME = 5 * 60 * 1000;

	// Number of milliseconds
	let timeA = {
		timeElapsed: MAX_TIME / 2,
		lastTime: Date.now(),
		count: 0
	};
	let timeB = {
		timeElapsed: MAX_TIME / 2,
		lastTime: Date.now(),
		count: 0
	};
	let currentPlayer: 'A' | 'B' | 'stop' = 'stop';

	setInterval(() => {
		if (currentPlayer === 'A') {
			timeA.timeElapsed -= Date.now() - timeA.lastTime;
			timeA.lastTime = Date.now();
		} else if (currentPlayer === 'B') {
			timeB.timeElapsed -= Date.now() - timeB.lastTime;
			timeB.lastTime = Date.now();
		}

		if (timeA.timeElapsed <= 0) {
			timeA.timeElapsed = 0;
			currentPlayer = 'B';
			timeB.lastTime = Date.now();
		}
		if (timeB.timeElapsed <= 0) {
			timeB.timeElapsed = 0;
			currentPlayer = 'A';
			timeA.lastTime = Date.now();
		}

		if (timeA.timeElapsed <= 0 && timeB.timeElapsed <= 0) {
			currentPlayer = 'stop';
			timeA.timeElapsed = MAX_TIME / 2;
			timeB.timeElapsed = MAX_TIME / 2;
			timeA.count = 0;
			timeB.count = 0;
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
			timeA.count = 0;
			timeB.count = 0;
			timeA.lastTime = Date.now();
			timeB.lastTime = Date.now();
			currentPlayer = mode;
		} else if (currentPlayer === 'A' && timeB.timeElapsed > 0) {
			timeA.count += 1;
			currentPlayer = 'B';
			timeB.lastTime = Date.now();
		} else if (currentPlayer === 'B' && timeA.timeElapsed > 0) {
			timeB.count += 1;
			currentPlayer = 'A';
			timeA.lastTime = Date.now();
		}
	}
</script>

<div class="w-screen h-screen flex flex-col lg:flex-row p-3 gap-3">
	<button
		class={`btn w-full h-full text-6xl font-bold rotate-180 lg:rotate-0 ${
			timeA.timeElapsed <= 0
				? 'variant-soft-error'
				: currentPlayer === 'A'
				? 'variant-soft-success'
				: 'variant-soft-surface'
		}`}
		on:click={() => switchPlayer('A')}
	>
		<div class="flex flex-col gap-2">
			{formatTime(timeA.timeElapsed)}
			<ProgressBar
				label="Progress Bar"
				track="bg-error-500"
				value={timeA.timeElapsed}
				max={MAX_TIME / 2}
			/>
			<p class="text-base">
				{timeA.count}
			</p>
		</div>
	</button>
	<button
		class={`btn w-full h-full text-6xl font-bold ${
			timeB.timeElapsed <= 0
				? 'variant-soft-error'
				: currentPlayer === 'B'
				? 'variant-soft-success'
				: 'variant-soft-surface'
		}`}
		on:click={() => switchPlayer('B')}
	>
		<div class="flex flex-col gap-2">
			{formatTime(timeB.timeElapsed)}
			<ProgressBar
				label="Progress Bar"
				track="bg-error-500"
				value={timeB.timeElapsed}
				max={MAX_TIME / 2}
			/>
			<p class="text-base">
				{timeB.count}
			</p>
		</div>
	</button>
</div>
