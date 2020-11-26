<script>
  import { getContext } from 'svelte';
  export let message;
	export let hasForm = false;
	export let onCancel = () => {};
	export let onOkay = () => {};

  const { close } = getContext('simple-modal');
	
	let value;
	let onChange = () => {};
	
	function _onCancel() {
		onCancel();
		close();
	}
	
	function _onOkay() {
		onOkay(value);
		close();
	}
	
	$: onChange(value)
</script>

<style>
	
	input {
		width: 100%;
	}
	
	.buttons {
		display: flex;
		justify-content: space-between;
	}

	article {
		padding : 2rem;
	}
</style>



<article>{@html message}</article>

{#if hasForm}
	<input
    type="text"
	  bind:value
	  on:keydown={e => e.which === 13 && _onOkay()} />
	<div class="buttons">
	</div>
{/if}

