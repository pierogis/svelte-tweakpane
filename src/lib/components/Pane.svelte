<script lang="ts">
	import { onDestroy, createEventDispatcher } from 'svelte';
	import { Pane, type TpPluginBundle } from 'tweakpane';

	export let container: HTMLElement;
	export let title: string | undefined = undefined;
	export let plugins: TpPluginBundle[] = [];

	let pane: Pane = new Pane({ container: container, title });

	plugins.forEach((plugin) => pane.registerPlugin(plugin));

	const dispatch = createEventDispatcher();
	pane.on('fold', () => {
		dispatch('fold');
	});

	onDestroy(() => {
		pane.dispose();
	});
</script>

<slot {pane} />
