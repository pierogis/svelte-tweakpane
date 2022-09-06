<script lang="ts">
	import { onDestroy } from 'svelte';
	import type { Readable } from 'svelte/store';

	import type { InputParams, Pane } from 'tweakpane';
	import type { FolderApi, InputBindingApi, TabPageApi, TpChangeEvent } from '@tweakpane/core';

	// generic for the bound value type of this input
	type T = $$Generic;

	export let parent: Pane | FolderApi | TabPageApi;
	export let paramsStore: Readable<{ [key: string]: T }>;
	export let onChange: (ev: TpChangeEvent<T>) => void;
	export let key: string;
	export let inputParams: InputParams | undefined = undefined;
	export let index: number | undefined = undefined;
	export let disabled: boolean = false;

	let params = $paramsStore;
	const inputApi: InputBindingApi<unknown, T> = parent
		.addInput(params, key, { ...inputParams, index, disabled })
		.on('change', onChange);

	$: inputApi.disabled = disabled;

	const inputElement = inputApi.element;
	// inputElement.style.maxWidth = '9rem';
	// inputElement.style.display = 'flex';

	onDestroy(() => {
		inputApi.dispose();
		parent.remove(inputApi);
	});
</script>

<slot {inputElement} />
