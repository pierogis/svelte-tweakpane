<script lang="ts">
	import { onMount } from 'svelte';

	import type { Pane } from 'tweakpane';
	import type { FolderApi, TpEvent, TabPageApi } from '@tweakpane/core';

	export let parent: Pane | FolderApi | TabPageApi;
	export let title: string;
	export let onClick: (ev: TpEvent) => void;
	export let index: number | undefined = undefined;

	let buttonElement: HTMLElement;

	onMount(() => {
		const buttonApi = parent.addButton({
			title,
			index
		});

		buttonApi.on('click', onClick);

		buttonElement = buttonApi.element;

		return () => {
			buttonApi.dispose();
			parent.remove(buttonApi);
		};
	});
</script>

<slot {buttonElement} />
