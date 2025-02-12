<script lang="ts">
	import ValueDiff from '$cmp/project/ValueDiff.svelte'
	import type { RegisterChunk, Register } from '$lib/languages/M68KEmulator'
	export let registers: Register[] = []
	import { settingsStore } from '$stores/settingsStore'
	import { Size } from 's68k'
	import { createEventDispatcher } from 'svelte'
	const dispatcher = createEventDispatcher<{
		registerClick: Register
	}>()
	export let size: Size = Size.Word
	export let style = ""
	let usesHex = !$settingsStore.values.useDecimalAsDefault.value
	$: usesHex = !$settingsStore.values.useDecimalAsDefault.value
	let chunks: RegisterChunk[][] = []
	$: chunks = registers.map(r => r.toSizedGroups(size))
</script>

<div class="registers" {style}>
	{#each registers as register, i (register.name)}
	<div class="row" style="gap: 0.2rem; flex: 1">
		<div class="register-wrapper">
			<div class="hover-register-value">
				{#if usesHex}
					{register.value}
				{:else}
					{register.value.toString(16).padStart(8, '0')}
				{/if}
			</div>
			<button class="register-name" on:click={() => dispatcher('registerClick', register)}>
				{register.name}
			</button>
		</div>
		<div class="register-hex">
			{#each chunks[i] as chunk}
				<ValueDiff 
					monospaced
					style="padding: 0.1rem"
					value={usesHex ? chunk.hex : `${chunk.value}`.padStart(chunk.groupSize, "0")}
					diff={usesHex ? chunk.prev.hex : `${chunk.prev.value}`.padStart(chunk.groupSize, "0")}
					hoverElementOffset={chunk.value !== chunk.valueSigned ? "-2.4rem" : "-1.2rem"}
				>
					<div class="column" slot="hoverValue">
						{#if chunk.value !== chunk.valueSigned}
							<div style="user-select: all;">
								{chunk.valueSigned}
							</div>
						{/if}
						<div style="user-select: all;">
							{usesHex ? `${chunk.value}`.padStart(chunk.groupSize, "0") : chunk.hex}
						</div>
					</div>
				</ValueDiff>
			{/each}
		</div>
	</div>

	{/each}
</div>

<style lang="scss">
	.registers {
		display: grid;
		background-color: var(--secondary);
		color: var(--secondary-text);
		padding: 0.5rem;
		border-radius: 0.5rem;
		grid-template-columns: auto;
		grid-template-rows: auto;
		flex-direction: column;
		gap: 0.2rem;
		align-items: center;
		min-width: 8.6rem;
		font-size: 1rem;
		flex: 1;
		@media screen and (max-width: 1000px) {
			width: unset;
		}
	}

	.hover-register-value {
		display: none;
		min-width: 100%;
		background-color: var(--tertiary);
		color: var(--tertiary-text);
		border-radius: 0.2rem;
		position: absolute;
		cursor: text;
		user-select: all;
		font-family: monospace;
		font-size: 1rem;
		box-shadow: rgba(0, 0, 0, 0.4) 0px 0px 6px;
		left: 1.6rem;
		z-index: 2;
		top: 0;
		height: 100%;
		padding: 0 0.7rem;
		align-items: center;
		font-weight: normal;
	}
	.register-wrapper {
		position: relative;
		&:hover .hover-register-value {
			display: flex;
		}
	}
	.register-name {
		font-weight: bold;
		padding: 0.2rem;
		border-radius: 0.2rem;
		border: none;
		width: 1.6rem;
		background: transparent;
		color: var(--secondary-text);
		cursor: pointer;
		&:hover {
			background-color: var(--accent2);
			color: var(--accent2-text);
		}
	}
	.register-hex {
		display: flex;
		justify-content: space-around;
		padding-left: 0.2rem;
		gap: 0.1rem;
		flex: 1;
		border-left: solid 0.1rem var(--tertiary);
	}
</style>
