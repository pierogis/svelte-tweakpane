<script lang="ts">
	import { writable } from 'svelte/store';

	import { Button, Folder, Input, Monitor, Pane, Tab } from '$lib';

	const title = 'pane';

	function handleFold() {
		console.log('fold');
	}

	let container: HTMLElement;

	const key = 'key';
	const paramsStore = writable({ [key]: 50 });
	const optParams = {};
</script>

<div class="outer">
	<div class="container" bind:this={container}>
		{#if container}
			<Pane {title} {container} let:pane on:fold={handleFold}>
				<Tab parent={pane} pages={[{ title: 'input/monitor' }, { title: 'folder' }]} let:tab>
					<Monitor
						parent={tab.pages[0]}
						{paramsStore}
						monitorParams={optParams}
						{key}
						interval={32}
						let:monitorElement
					/>
					<Input
						parent={tab.pages[0]}
						{paramsStore}
						inputParams={optParams}
						onChange={(ev) => console.log(ev.value)}
						{key}
						let:inputElement
					/>

					<Folder tab={{ api: tab, pageIndex: 1 }} title={'folder'} let:folder>
						<Button title={'button'} parent={folder} onClick={(ev) => console.log('click')} />
					</Folder>
				</Tab>
			</Pane>
		{/if}
	</div>
</div>

<style>
	.container {
		display: flex;
	}

	.outer {
		display: flex;
		place-content: center;

		position: absolute;
		top: 50%;
		left: 50%;

		width: 20rem;
		height: 10rem;

		padding: 6rem;
		background-color: rgb(180, 180, 180);

		transform: translate(-50%, -50%);

		border-radius: 0.5rem;
	}
</style>
