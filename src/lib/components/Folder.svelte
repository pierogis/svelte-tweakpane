<script lang="ts">
	import { onMount } from 'svelte';

	import type { FolderApi, Pane, TabApi, TabPageApi } from 'tweakpane';

	export let pane: Pane | undefined = undefined;
	export let folder: FolderApi | undefined = undefined;
	export let tab: { api: TabApi; pageIndex: number } | undefined = undefined;

	export let title: string;
	export let index: number | undefined = undefined;

	let parentApi: FolderApi | TabPageApi;
	let parentRackElement: HTMLElement;

	if (pane) {
		parentApi = pane;
		parentRackElement = parentApi.controller.rackController.element;
	} else if (folder) {
		parentApi = folder;
		parentRackElement = parentApi.controller.rackController.element;
	} else if (tab) {
		parentApi = tab.api.pages[tab.pageIndex];
		parentRackElement = parentApi.controller.rackController.element;
	} else {
		throw 'pane or parentFolder or tab must be provided';
	}

	const folderApi = parentApi.addFolder({
		title,
		index
	});

	$: folderApi.title = title;

	const container = folderApi.element;

	let folderContainer: HTMLElement;

	onMount(() => {
		folderContainer.append(container);
		parentRackElement.append(folderContainer);

		return () => {
			parentApi.remove(folderApi);
		};
	});
</script>

<slot folder={folderApi} {folderContainer} />

<div class="folder-container" bind:this={folderContainer} />

<style>
	.folder-container {
		display: flex;
		place-items: center;
	}
</style>
