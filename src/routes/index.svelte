<script context="module" lang="ts">
</script>

<script lang="ts">
	import { onMount } from 'svelte/internal';

	let peer, conn
	onMount(async () => {
		const myId = location.hash === '#initiator'?'1':'2'
		const partnerId = location.hash === '#initiator'?'2':'1'
		peer = new Peer(myId, {
      host: 'h2949981.stratoserver.net',
      port: 9001,
      path: '/myapp'
    });;
		conn = peer.connect(partnerId);
		conn.on('open', () => {
			conn.send('hi');
		});

    peer.on('connection', function(conn) {
      conn.on('data', function(data){
        console.log(data);
      });
    });
  });

	const startStreaming = async () => {
		const stream = await navigator.mediaDevices.getUserMedia({
			video: true,
			audio: true
		});
		peer.addStream(stream);
	};
  const sendHi =()=>{conn.send('hihihihiihihi')} 
</script>

<svelte:head>
	<title>Home</title>
	<script src="https://unpkg.com/peerjs@1.4.5/dist/peerjs.min.js"></script>
</svelte:head>

<div class="p-4">
  <button on:click={startStreaming}  class="btn btn-primary">
    Start Streaming
  </button>

  <button on:click={sendHi} class="btn btn-secondary">
    Send 'Hi'
  </button>
	<video />
</div>

<style>
</style>
