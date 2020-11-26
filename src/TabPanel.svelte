<script>
	import { getContext } from 'svelte';
	import { TABS } from './Tabs.svelte';
	import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

	const panel = {};
	const { registerPanel, selectedPanel } = getContext(TABS);

	registerPanel(panel);

</script>

{#if $selectedPanel === panel}
	<slot  ></slot>
{/if}
