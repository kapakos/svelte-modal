<script>
import { fly } from 'svelte/transition';
	
export let visible = false;
export let showFooter = true;
export let showHeader = true;
export const open = () => {
	visible = true;
}
export const close = () => {
	visible = false;
}

$: {
	if(visible){	
		const backdrop = document.createElement('div');
		backdrop.setAttribute('style', backdropStyle.trim());
		backdrop.id = 'modal_backdrop';
		document.body.appendChild(backdrop);	
	} else {
		const del = document.getElementById('modal_backdrop');
		if(del){
			del.remove();
		}		
	}
}

const backdropStyle = `
	position: fixed;
	background: rgb(0.3,0.3,0.3, 0.3);
	z-index: 5;
	left: 0;
	right: 0;
	top: 0;
	bottom: 0;
`;

</script>
<style>
	.modal {
		position: fixed;
		left: 10%;
		right: 10%;
		top: 10%;
		bottom: 10%;
		z-index: 10;
		background: white;
		box-shadow: 0px 0px 10px rgb(0.5,0.5,0.5, 0.5);
	}
	.modal_footer {
		border-top: 1px solid lightGrey;
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		padding: 10px;
		display: flex;
  	align-items: center;
		justify-content: center;
	}
	.modal_header {
		display: flex;
  	align-items: center;
		left: 0;
		right: 0;
		top:0;
		height: 50px;
		padding-left: 20px;
		box-shadow: 0px 0px 5px rgb(0.5,0.5,0.5, 0.5);
	}
	.modal_main {
		padding: 20px;
	}
	.modal__footer-button {
		width: 100px;
		background: white;
		border-radius: 0;
		border: 0;
		box-shadow: 0 2px 5px 0 rgba(0,0,0,0.16);		
		margin: 0;
	}
	.modal__footer-button:first-child {
		margin-right: 50px;
	}
</style>

{#if visible}
<div transition:fly="{{ y: -100, duration: 300 }}" class="modal">
	<div class="modal_content">
		{#if showHeader}
		<div class="modal_header">
			<slot name="header_logo" />
			<slot name="header_title" />
		</div>
		{/if}
		<div class="modal_main">
			<slot>...Enter some content</slot>
		</div>
		{#if showFooter}
		<div class="modal_footer">
			<slot name="footer">
				<button class="modal__footer-button" on:click={close}>
					Accept
				</button>
				<button class="modal__footer-button" on:click={close}>
					Cancel
				</button>
			</slot>
		</div>
		{/if}
	</div>
</div>
{/if}
