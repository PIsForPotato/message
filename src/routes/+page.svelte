<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<script lang="ts">
    import {io} from "socket.io-client";
    const socket = io("sockets.hosted.coasterfan5.com")
    let messages: Message[] = []
    let message = '';

    type Message = {
        author: string,
        message: string,
        server: {
            online: number,
            userList: String[]
        }
    }

    socket.on("connect", () => {
        socket.emit("setName", "Luigi from hit game Super Mario Wii U");
    })

    socket.on("message", message => {
        messages = [...messages, message]
    })

    function submit() {
        socket.emit("message", message)
        message = '';
    }
</script>

<input bind:value={message}/>
<button on:click={submit}>Submit</button>

<div>
    {#each messages as message}
        <p>{message.author}: {message.message}</p>
    {/each}
</div>


