<script context="module">
	export const TABS = {};
</script>

<script>
	import { setContext, onDestroy } from 'svelte';
	import { writable } from 'svelte/store';
	import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

	const tabs = [];
	const panels = [];
	const selectedTab = writable(null);
	const selectedPanel = writable(null);

	setContext(TABS, {
		registerTab: tab => {
			tabs.push(tab);
			selectedTab.update(current => current || tab);
			
			onDestroy(() => {
				const i = tabs.indexOf(tab);
				tabs.splice(i, 1);
				selectedTab.update(current => current === tab ? (tabs[i] || tabs[tabs.length - 1]) : current);
			});
		},

		registerPanel: panel => {
			panels.push(panel);
			selectedPanel.update(current => current || panel);
			
			onDestroy(() => {
				const i = panels.indexOf(panel);
				panels.splice(i, 1);
				selectedPanel.update(current => current === panel ? (panels[i] || panels[panels.length - 1]) : current);
			});
		},

		selectTab: tab => {
			const i = tabs.indexOf(tab);
			selectedTab.set(tab);
			selectedPanel.set(panels[i]);
		},

		selectedTab,
		selectedPanel
	});
	/*function onSubmit(e){
		dispatch('submit', e)
	}*/
</script>

<div class="tabs">
	<slot  ></slot><!--onSubmit={onSubmit}-->
</div>

<style>
	.tabs{
		margin: 5rem;
		border: 1px solid #e0e0e0;
		border-radius: 0.2rem;
		box-shadow: 5px 5px 10px grey;
	}
</style>