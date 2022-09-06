<script lang="ts">
	import { onDestroy } from 'svelte';

	import type { TabApi } from '@tweakpane/core';

	export let tab: TabApi;
	export let title: string;

	export let index: number;

	const page = tab.addPage({ title, index });

	onDestroy(() => {
		tab.removePage(index);
	});
</script>

<slot {page} />
