<script lang="ts">
	import { onDestroy } from 'svelte';

	import type { Pane } from 'tweakpane';
	import type { BaseBladeParams, FolderApi, TabPageApi } from '@tweakpane/core';

	// generic for the bound value type of this monitor
	type T = $$Generic;

	export let parent: Pane | FolderApi | TabPageApi;
	export let bladeParams: BaseBladeParams = {};

	const bladeApi = parent.addBlade(bladeParams);

	onDestroy(() => {
		bladeApi.dispose();
		parent.remove(bladeApi);
	});
</script>

<slot {bladeApi} />
