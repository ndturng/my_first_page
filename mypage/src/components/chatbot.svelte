<script>
    // @ts-nocheck
    import {autofocus} from '../untils/autofocus'
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
    ];
    let comments = [];
   
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
    
    <div>
        {#each comments as comment}
            <article>
                <span>{comment.text}</span>
            </article>
        {/each}
    </div>
    <input use:autofocus on:keydown={handleKeyDown}>
</div>




<style>
    
</style>