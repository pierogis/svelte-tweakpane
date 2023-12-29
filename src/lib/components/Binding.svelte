<script lang="ts">
	import { onDestroy } from 'svelte';
	import type { Readable } from 'svelte/store';

	import type { Pane } from 'tweakpane';
	import type { BindingParams, FolderApi, TabPageApi, TpChangeEvent } from '@tweakpane/core';

	// generic for the bound value type of this monitor
	type T = $$Generic;

	export let parent: Pane | FolderApi | TabPageApi;
	/**
	 * contains the param that this binding will display
	 */
	export let paramsStore: Readable<{ [key: string]: T }>;
	export let onChange: ((ev: TpChangeEvent<T>) => void) | undefined = undefined;

	export let key: string;
	/**
	 * options for this binding, not to be confused with tweakpane params
	 *
	 * provide `readonly: true` to get a monitor
	 */
	export let bindingParams: BindingParams | undefined = undefined;

	const params = $paramsStore;
	const bindingApi = parent.addBinding(params, key, bindingParams);
	if (onChange) bindingApi.on('change', onChange);

	let fired = false;
	paramsStore.subscribe((newParams) => {
		if (fired) {
			if (newParams[key] != params[key]) {
				params[key] = newParams[key];
			}
		}
		fired = true;
	});

	onDestroy(() => {
		bindingApi.dispose();
		parent.remove(bindingApi);
	});
</script>

<slot {bindingApi} />
