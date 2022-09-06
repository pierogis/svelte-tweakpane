# svelte-tweakpane

## installation

```bash
npm i --save-dev @pierogis/svelte-tweakpane
```

## usage

```svelte
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
```

```svelte
<div bind:this={container}>
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
```

```svelte
<style>
	div {
		display: flex;
	}
</style>
```

the container defines the size of the inner pane

also see

```ts
import { Button, Folder, Input, Monitor, Pane, Tab } from "@pierogis/svelte-tweakpane"
```

## development

install dependencies with `npm install`
start a development server with:

```bash
npm run dev
```

## building

to create a production package with:

```bash
npm run build
```

preview the production build with `npm run preview`.