<script lang="ts">
	import { beforeUpdate, afterUpdate, SvelteComponent } from 'svelte';

	let comments : {author:string, text:string, placeholder:boolean}[]= [
		// { author: 'sino', text: "hello, world!", placeholder: false }
	];

let div : HTMLDivElement;
let theInput : HTMLInputElement;
let autoscroll : boolean;

beforeUpdate(() => {
	autoscroll = div && (div.offsetHeight + div.scrollTop) > (div.scrollHeight - 20);
});

afterUpdate(() => {
	if (autoscroll) div.scrollTo(0, div.scrollHeight);
});

function postMessage(text:string, inputField:HTMLInputElement) {
	comments = comments.concat({
				author: 'user',
				text,
				placeholder: false
			});

			inputField.value = '';

			const roll = Math.random() < 0.5;
			let reply = ""
			if(roll) reply = "si"
			else reply = "no"

			setTimeout(() => {
				comments = comments.concat({
					author: 'sino',
					text: '...',
					placeholder: true
				});

				setTimeout(() => {
					comments = comments.filter(comment => !comment.placeholder).concat({
						author: 'sino',
						text: reply,
						placeholder: false
					});
				}, 500 + Math.random() * 500);
			}, 200 + Math.random() * 200);
}

function handleKeydown(event : KeyboardEvent) {
		if (event.key === 'Enter') {
			const element = event.target as HTMLInputElement
			const text = element.value;
			if (!text) return;

			postMessage(text, element)
		}
	}

	function handleSendClick(_event: Event){
		const text = theInput.value;
		if(!text || text.length < 1) return;
		postMessage(text, theInput)
	}
</script>

<svelte:head>
	<title>Sino 2023</title>
	<meta name="description" content="Sino online" />
</svelte:head>

<div class="chat-container">
	<div class="chat">
		<h1 class="top-bar">Sino '23</h1>
	
		<div class="scrollable" bind:this={div}>
			{#each comments as comment}
				<article class={comment.author}>
					<span class="msg-bubble">{comment.text}</span>
				</article>
			{/each}
		</div>
	
	</div>
	<div class="bottom-bar">
		<div class="input-line">
			<input class="input-field" on:keydown={handleKeydown}  bind:this={theInput}>
			<button class="send-button" on:click={handleSendClick}>Send</button>
		</div>
	</div>
</div>

<style>
	.top-bar {
		margin-top:0px;
		padding:15px;
		box-shadow: 0px 2px 10px rgba(50,50,50,0.2);
	}

	.bottom-bar {
		display: flex;
		justify-content: center;
		width: 100%;
		box-shadow: 0px -2px 10px rgba(50,50,50,0.2);
		padding: 20px 0px;
	}

	.input-line {
		display: flex;
		align-items: center;
		justify-content:center;
		flex-grow: 1;
		max-width: 480px;
		margin-left: auto;
		margin-right: auto;
	}

	.chat-container {
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: end;
		align-items: center;
	}
	.chat {
		display: flex;
		flex-direction: column;
		height: 100%;
		width: 100%;
		overflow: hidden;
	}

	.scrollable {
		flex: 1 1 auto;
		margin: 0 0 0.5em 0;
		overflow-y: auto;
	}

	article {
		margin: 0.5em 0;
	}

	.user {
		text-align: right;
	}

	span {
		padding: 0.5em 1em;
		display: inline-block;
	}

	.msg-bubble{
		box-shadow: 0px 2px 10px rgba(50,50,50,0.2);
		margin: 0px 20px
	}

	.sino span {
		background-color: #eee;
		border-radius: 1em 1em 1em 0;
	}

	.user span {
		background-color: #0074D9;
		color: white;
		border-radius: 1em 1em 0 1em;
		word-break: break-all;
	}

	.input-field {
		flex-grow: 1;
		margin-left:20px;
		border: none;
		border-radius: 0.25em;
		box-shadow: 0px 0px 10px rgba(50,50,50,0.2);
	}

	.send-button {
		margin: 0px 20px 0px 20px;
		color:white;
		background: linear-gradient(#3b99eb 0%, #0074D9 50%);
		border: none;
		border-radius: 0.25em;
	}

	.send-button:hover{
		background: linear-gradient(#3385cd 0%, #075497 50%);;
	}

	.send-button:active{
		color:black;
		background: linear-gradient(#93c6f3 0%, #b8c1ca 50%);
	}
</style>
