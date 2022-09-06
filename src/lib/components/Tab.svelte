<script lang="ts">
	import { createEventDispatcher, onDestroy } from 'svelte';

	import type { TpUpdateEvent, TpTabSelectEvent, TpChangeEvent } from '@tweakpane/core';
	import type { FolderApi, Pane, TabPageApi } from 'tweakpane';

	export let parent: Pane | FolderApi | TabPageApi;

	export let pages: { title: string }[];
	export let disabled: boolean | undefined = undefined;
	export let index: number | undefined = undefined;

	const tabApi = parent.addTab({
		pages,
		disabled,
		index
	});

	const dispatch = createEventDispatcher<{
		change: TpChangeEvent<unknown>;
		select: TpTabSelectEvent;
		update: TpUpdateEvent<unknown>;
	}>();
	tabApi.on('change', (event) => dispatch('change', event));
	tabApi.on('select', (event) => dispatch('select', event));
	tabApi.on('update', (event) => dispatch('update', event));

	$: pages.forEach((page, i) => {
		tabApi.pages[i].title = page.title;
	});

	const tabElement = tabApi.element;

	onDestroy(() => {
		parent.remove(tabApi);
	});
</script>

<slot {tabElement} tab={tabApi} />
