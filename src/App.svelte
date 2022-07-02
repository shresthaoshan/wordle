<script lang="ts">
	import Fields from "./lib/Fields.svelte";

	const answer = "PLANE";

	let res: Array<Result[]> = [];
	let inputs: String[] = new Array(5).fill("");

	const updateAnswer = (value: string, index: number) => {
		inputs[index] = value;
	};

	const onRowSubmit = () => {
		const r: Result[] = new Array(5).fill(0);
		for (let i = 0; i < 5; i++) {
			if (answer[i] === inputs[i].toString()) {
				r[i] = 2;
				continue;
			}
			if (answer.includes(inputs[i].toString())) {
				r[i] = 1;
				continue;
			}
			r[i] = 0;
		}
		inputs = new Array(5).fill("");
		res[activeRow] = r;
	};

	$: activeRow = Math.min(res.length, 4);
</script>

<main>
	<h2>WORDLE</h2>

	<!-- debug -->
	<p>
		{activeRow}
	</p>

	<!-- fields below -->
	{#each new Array(5).fill(0) as _, idx}
		<Fields active={activeRow === idx} results={res[idx]} {updateAnswer} />
	{/each}

	<!-- controls -->
	<div class="control">
		<button on:click={onRowSubmit}>Submit</button>
	</div>
</main>

<style global>
	:root {
		font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
			sans-serif;
	}

	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
	}

	body {
		--ff: "Metrophobic", sans-serif;
		--fail-color: #1d1d1d;
		--exist-color: #606060;
		--success-color: #5a881d;
		font-family: var(--ff);
	}

	button {
		cursor: pointer;
		border: none;
		background-color: #333;
		color: white;
		transition: all 200ms ease-in-out;
	}
	button:active {
		background-color: #777;
	}

	.control {
		padding: 20px;
	}
	.control button {
		padding: 15px 25px;
		font-family: var(--ff);
		font-size: 15px;
		font-weight: 600;
		text-transform: uppercase;
	}
</style>
