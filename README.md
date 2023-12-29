# svelte-tweakpane

svelte components wrapping [tweakpane](https://github.com/cocopon/tweakpane)

## installation

```bash
npm i --save-dev @pierogis/svelte-tweakpane
# or
pnpm add -D @pierogis/svelte-tweakpane
```

## usage

```svelte
<script lang="ts">
	import { writable } from 'svelte/store';

	import { Binding, Blade, Button, Folder, Pane, Tab } from '@pierogis/svelte-tweakpane';

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

<div bind:this={container}>
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
```

the container (from `bind:this`) defines the size of the pane

### exports

```ts
import { Blade, Binding, Button, Folder, Pane, Tab } from "@pierogis/svelte-tweakpane"
```

### theming

style in a `+layout.svelte` like so (see [theming](https://cocopon.github.io/tweakpane/theming/) for full list of observed css variables)

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

clone the repo
install dependencies with `pnpm i`
start a development server with:

```bash
pnpm dev
```

## release

- push with necessary changesets
- gh action will make a version PR
- create prerelease (if needed)

```
pnpm changeset pre enter next
pnpm changeset version
pnpm changeset publish
```

- merge changeset version pr and pull
- publish release

```
pnpm changeset publish
```
