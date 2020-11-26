<script>
	import { getContext } from 'svelte';
	import { TABS } from './Tabs.svelte';

	const tab = {};
	const { registerTab, selectTab, selectedTab } = getContext(TABS);
	export let label;

	registerTab(tab);
</script>

<style>
	li {
		display: inline-block;
	}
	button {
		background: none;
		border: 1px solid transparent	;
		border-bottom: 2px solid white;
		border-radius: 0;
		margin: 0;
		color: #ccc;
		padding:1rem;
	}
	button:focus {
		border: 1px solid blue;
	}
	
	.selected {
		border-bottom: 2px solid teal !important;
		color: #333;
	}
</style>

<li>
<button aria-label={label} class:selected="{$selectedTab === tab}" on:click="{() => selectTab(tab)}" >
	<slot></slot>
</button>
</li>