<script>
	import Tabs  from './Tabs.svelte';
	import TabList  from './TabList.svelte';
	import TabPanel  from './TabPanel.svelte';
	import Tab  from './Tab.svelte';
  import { getContext } from 'svelte';
	import { fly } from 'svelte/transition';
	import Dialog from './Dialog.svelte';
	import CloseButton from './CloseButton.svelte';
	
	const { open } = getContext('simple-modal');

	let email = '';
	let emailHasErrors = false;
	let name = '';
	let nameHasErrors = false;
	let valid = false;
	let hasSubmitted = false;
	let submitted = false;

	const showDialog = () => {
		open(
			Dialog,
			{
				message: `<h2>Lorem ipsum dolor</h2>
				<p>Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt.</p>`,
				hasForm: false,
			},
			{
				closeButton: true,
    		closeOnEsc: true,
    		closeOnOuterClick: true,
			}
	  );
	};

	const onChange = (e) => {
		switch(e.target.name){
			case 'name' :
				name = e.target.value;
				break;
			case 'email' :
				email = e.target.value;
				break;
		}
		validate();
	}

	const validate = () => {
		let tempValid = true;
		if(name === ""){
			tempValid = false;
			nameHasErrors = true;
		}else{
			nameHasErrors = false;
		}
		if(email === ""){
			tempValid = false;
			emailHasErrors = true;
		}else{
			emailHasErrors = false;
		}
		valid = tempValid;
	}

	const onSubmit = (e) => {
		hasSubmitted = true;
		validate();
		if(valid){
			submitted = true
		}
	}

</script>
	{#if submitted === false}
	<Tabs >
		<TabList>
			<Tab label="Access dialog tab">Dialog</Tab>
			<Tab label="Access form tab">Form</Tab>
		</TabList>

		<TabPanel >
			<div class="tabPanel">
				<button on:click={showDialog}>Open a modal</button>
			</div>
		</TabPanel>

		<TabPanel  >
			<div class="tabPanel">
				<form class="form">
					<!--<label for="name" aria-label={nameHasErrors && hasSubmitted?"The name is invalid":"name"}>Name</label>-->
					<input placeholder="Name*" type="text" aria-label={nameHasErrors && hasSubmitted?"The name is invalid":"name"} id="name" name="name" class:error={nameHasErrors && hasSubmitted} on:input={onChange}/>
					<!--<label for="email" aria-label={emailHasErrors && hasSubmitted?"The email is invalid":"email"}>E-mail</label>-->
					<input placeholder="E-mail*" type="text" aria-label={emailHasErrors && hasSubmitted?"The email is invalid":"email"} id="email" name="email" class:error={emailHasErrors && hasSubmitted} on:input={onChange} />
					<button type="button" on:click={onSubmit} aria-label={valid || !hasSubmitted ?"Submit":"Fill every field before submitting"}>Submit</button>
				</form>
			</div>
		</TabPanel>
	</Tabs>
	{:else}
		<div class="thankYou">
			<h1>Thank you</h1>
			<p>You completed the registration process.</p>
		</div>
	{/if}

<style>
.thankYou{
	padding : 2rem;
	margin: 5rem;
	border: 1px solid #e0e0e0;
	border-radius: 0.2rem;
	box-shadow: 5px 5px 10px grey;
}
.error {
	border-color: #e00000;
}
.tabPanel {
	padding: 2rem;
	min-height: 10rem;
	min-width: 10rem;
}	
.tabPanel button:focus {
	border: 1px solid blue;
}
.form {
	display: grid;
	grid-template-columns: 100%;
	grid-column-gap: 10px;
}
.tabPanel button{
	max-width: 10rem;
	right:0;
}
</style>