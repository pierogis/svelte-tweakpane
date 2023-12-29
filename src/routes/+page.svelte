<script lang="ts">
	import { writable } from 'svelte/store';

	import { Binding, Blade, Button, Folder, Pane, Tab } from '$lib';

	const title = 'pane';

	function handleFold() {
		console.log('fold');
	}

	let container: HTMLElement;

	const key = 'key';
	const paramsStore = writable({ [key]: 50 });
	const monitorParams = {
		readonly: true,
		interval: 32
	};
	const inputParams = {};
</script>

<div class="outer">
	<div class="container" bind:this={container}>
		{#if container}
			<Pane {title} {container} let:pane on:fold={handleFold}>
				<Tab parent={pane} pages={[{ title: 'input/monitor' }, { title: 'folder' }]} let:tab>
					<Binding
						parent={tab.pages[0]}
						{paramsStore}
						bindingParams={monitorParams}
						{key}
						let:bindingApi
					/>
					<Binding
						parent={tab.pages[0]}
						{paramsStore}
						bindingParams={inputParams}
						onChange={(ev) => console.log(ev.value)}
						{key}
						let:bindingApi
					/>
					<Blade
						parent={tab.pages[0]}
						bladeParams={{
							view: 'slider',
							label: 'brightness',
							min: 0,
							max: 1,
							value: 0.5
						}}
						let:bladeApi
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
	.outer {
		display: flex;
		justify-content: center;

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
	.container {
		width: 20rem;
	}
</style>
