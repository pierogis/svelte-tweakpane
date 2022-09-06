<script lang="ts">
	import { onDestroy } from 'svelte';
	import type { Readable } from 'svelte/store';

	import type { MonitorParams, Pane } from 'tweakpane';
	import type { FolderApi, MonitorBindingApi, TabPageApi } from '@tweakpane/core';

	// generic for the bound value type of this monitor
	type T = $$Generic;

	export let parent: Pane | FolderApi | TabPageApi;
	export let paramsStore: Readable<{ [key: string]: T }>;
	export let key: string;
	export let monitorParams: MonitorParams | undefined = undefined;
	export let index: number | undefined = undefined;
	export let interval: number | undefined = undefined;

	const params = $paramsStore;
	const monitorApi: MonitorBindingApi<T> = parent.addMonitor(params, key, {
		...monitorParams,
		index,
		interval
	});

	const monitorElement = monitorApi.element;
	// monitorElement.style.maxWidth = '9rem';
	// monitorElement.style.display = 'flex';

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
		parent.remove(monitorApi);
	});
</script>

<slot {monitorElement} />
