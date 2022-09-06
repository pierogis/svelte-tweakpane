# svelte-tweakpane

svelte components wrapping [tweakpane](https://github.com/cocopon/tweakpane)

## installation

```bash
npm i --save-dev @pierogis/svelte-tweakpane
```

## usage

```svelte
<script lang="ts">
	import { writable } from 'svelte/store';

	import { Button, Folder, Input, Monitor, Pane, Tab } from '@pierogis/svelte-tweakpane';

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

```ts
import { Button, Folder, Input, Monitor, Pane, Tab } from "@pierogis/svelte-tweakpane"
```

style in +layout.svelte like so (see [theming](https://cocopon.github.io/tweakpane/theming/) for full list of observed css variables)
```svelte
<style>
	:global(:root) {
		--tp-base-background-color: #color;
		--tp-base-shadow-color: #color;

		/* button */
		--tp-button-background-color: #color;
		--tp-button-background-color-active: #color;
		--tp-button-background-color-focus: #color;
		--tp-button-background-color-hover: #color;
		--tp-button-foreground-color: #color;

		/* label */
		--tp-label-background-color: #color;
		--tp-label-foreground-color: #color;

		/* input */
		--tp-input-background-color: #color;
		--tp-input-background-color-active: #color;
		--tp-input-background-color-focus: #color;
		--tp-input-background-color-hover: #color;
		--tp-input-foreground-color: #color;

		--tp-container-background-color: #color;
		--tp-container-background-color-active: #color;
		--tp-container-background-color-focus: #color;
		--tp-container-background-color-hover: #color;
		--tp-container-foreground-color: #color;
	}
</style>
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
