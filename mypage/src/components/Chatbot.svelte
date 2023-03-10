<script>
    // @ts-nocheck
    import {autofocus} from '../untils/autofocus'
    import { afterUpdate } from 'svelte';

    let div;
	let autoscroll = true;
    let comments = [];

    
    afterUpdate(() => {
		if (autoscroll) div.scrollTo(0, div.scrollHeight);
	});

    const chat_chunks = [
        {
            id: 1,
            text: 'Hello, I am a chatbot. How are you today?',
        },
        {
            id: 2,
            text: 'I am feeling great today',
        },
        {
            id: 3,
            text: 'That is amazing',
        },
        {
            id: 4,
            text: 'Thank you for talking to me.',
        },
        {
            id: 5,
            text: 'Please go on',
        }
    ];

    function handleKeyDown(event) {
        if (event.key === 'Enter') {
            const text = event.target.value;
            if (!text) return;

            comments = comments.concat({ author: 'user', text });

            event.target.value = '';

            let num_random = Math.floor(Math.random() * chat_chunks.length);

            let reply = chat_chunks[num_random].text;

			setTimeout(() => {
				comments = comments.concat({
					author: 'bot',
					text: '...',
					placeholder: true
				});

				setTimeout(() => {
					comments = comments.filter(comment => !comment.placeholder).concat({
						author: 'bot',
						text: reply
					});
				}, 500 + Math.random() * 500);
			}, 200 + Math.random() * 200);
		}
	};
</script>

<div class="chat">
    <h1>Chat with Stupid Bot</h1>
    
    <div class="scrollable" bind:this={div}>
        {#each comments as comment}
            <article class={comment.author}>
                <span>{comment.text}</span>
            </article>
        {/each}
    </div>
    <input use:autofocus on:keydown={handleKeyDown}>
</div>
    
<style>
    h1 {
        text-align: center;
    }

    /* creat a box at the center of page to chat */
    .chat {
        display: flex; 
    flex-direction: column; 
    height: 100%; 
    max-width: 400px; 
    max-height: 600px; 
    margin: 0 auto; 
    border: 1px transparent; 
    padding: 5px; 
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
    overflow-x:hidden;
    }

    .scrollable {
		flex: 1 1 auto;
		border-top: 1px solid #eee;
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
    
    .bot span {
		background-color: #eee;
		border-radius: 1em 1em 1em 0;
	}

	.user span {
		background-color: #0074D9;
		color: white;
		border-radius: 1em 1em 0 1em;
		word-break: break-all;
	}
</style>