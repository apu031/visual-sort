<script lang="ts">
	import { onMount } from 'svelte';
	import { get, writable } from 'svelte/store';
	import type { Writable } from 'svelte/store';

	const numOfEntries = 20;

	const swapTimerMs = 500;

	function genRandomArray(arrLength: number) {
		return [...Array(arrLength)].map(() => Math.floor(Math.random() * (10 * arrLength)));
	}

	// Returns a Promise that resolves after "ms" Milliseconds
	const timer = (ms) => new Promise((res) => setTimeout(res, ms));

	async function bubbleSort(arr: Writable<number[]>) {
		const cpyArr = get(arr);
		const { length } = cpyArr;

		const swap = (a: number, b: number): void => {
			cpyArr[a] = cpyArr[a] + cpyArr[b];
			cpyArr[b] = cpyArr[a] - cpyArr[b];
			cpyArr[a] = cpyArr[a] - cpyArr[b];
		};

		for (let x = 0; x < length - 1; x++) {
			for (let y = 0; y < length - 1 - x; y++) {
				const [currentIndex, nextIndex] = [y, y + 1];
				let curr = document.getElementById(cpyArr[currentIndex].toString() + '_div');
				let next = document.getElementById(cpyArr[nextIndex].toString() + '_div');
				curr.style.backgroundColor = 'green';
				next.style.backgroundColor = 'green';
				await timer(swapTimerMs);
				if (cpyArr[currentIndex] > cpyArr[nextIndex]) {
					swap(currentIndex, nextIndex);
					arr.set(cpyArr);
				}
				curr.style.backgroundColor = 'blue';
				next.style.backgroundColor = 'blue';
			}
		}
	}

	const unsortedArr = writable<number[]>();

	unsortedArr.set(genRandomArray(numOfEntries));

	onMount(() => {
		bubbleSort(unsortedArr);
	});
</script>

<div class="div-canvas">
	{#each $unsortedArr as arr}
		<div
			id={arr.toString() + '_div'}
			class="bar"
			style={'width: 20px; height: ' + ((arr + 1) * 2).toString() + 'px;'}
		>
			<p class="bar-text">{arr}</p>
		</div>
	{/each}
</div>

<hr />

<button class="btn"
	on:click={() => {
		unsortedArr.set(genRandomArray(numOfEntries));
		bubbleSort(unsortedArr);
	}}>Update</button
>

<style>
	.div-canvas {
		display: flex;
		-webkit-transform: rotate(90deg); /* to support Safari and Android browser */
		-ms-transform: rotate(180deg); /* to support IE 9 */
		transform: rotate(180deg);
		margin-bottom: 10px;
		justify-content: center;
	}
	.bar {
		padding: 10px;
		margin: 10px;
		background: blue;
	}

	.bar-text {
		-webkit-transform: rotate(90deg); /* to support Safari and Android browser */
		-ms-transform: rotate(180deg); /* to support IE 9 */
		transform: rotate(180deg);
	}

	.btn {
		background: cadetblue;
		border-radius: 5px;
		padding: 10px;
		font-weight: bold;
	}
</style>
