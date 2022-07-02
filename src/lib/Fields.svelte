<script lang="ts">
	export let active: boolean = false;
	export let results: Array<Result> = undefined;
	export let updateAnswer: (val: string, idx: number) => void;

	const resultClasses = ["fail", "exists", "right"];
	const acceptable = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

	const fields = document.getElementsByClassName("field active");

	let activeCol = 0;

	const onUpdate =
		(index: number) =>
		(
			ev: Event & {
				currentTarget: EventTarget & HTMLInputElement;
			}
		) => {
			const val = ev.currentTarget.value.toUpperCase();
			if (!val.length) {
				if (index > 0) (fields.item(index - 1).children[0] as HTMLInputElement).focus();
				if (activeCol > 0) {
					activeCol -= 1;
				}
			} else if (acceptable.includes(val)) {
				// inputs[index] = val;
				updateAnswer(val, index);
				if (index < 4) (fields.item(index + 1).children[0] as HTMLInputElement).focus();
				if (activeCol < 4) {
					activeCol += 1;
				}
			} else {
				console.log("Nothing to do");
			}
		};
</script>

<section class="fields">
	{#each new Array(5).fill(0) as _, i}
		<div class={active ? "field active" : "field"}>
			<input
				class={`${i} ${results ? resultClasses[results[i]] : ""}`}
				type="text"
				disabled={!active}
				maxlength="1"
				on:input={onUpdate(i)}
			/>
		</div>
	{/each}
</section>

<style lang="scss">
	.fields {
		padding: 10px 20px;

		display: flex;
		justify-content: center;
		align-items: center;
		gap: 20px;

		.field {
			width: 30px;
			height: 30px;

			input {
				width: 100%;
				height: 100%;
				text-align: center;
				font-family: var(--ff);
				font-size: 18px;
				font-weight: 600;
				line-height: 1;
				text-transform: uppercase;
				border: 1px solid grey;

				&.fail {
					--color: var(--fail-color);
					background-color: var(--color);
					border-color: var(--color);
				}
				&.exists {
					--color: var(--exist-color);
					background-color: var(--color);
					border-color: var(--color);
				}
				&.right {
					--color: var(--success-color);
					background-color: var(--success-color);
					border-color: var(--color);
				}
                &.fail, &.exists, &.right {
                    color: white;
                }
			}
		}
	}
</style>
