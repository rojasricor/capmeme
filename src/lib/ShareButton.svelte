<script lang="ts">
	import { canvas } from './stores';

	const getImage = () => {
		const imageURL = $canvas.toDataURL();
		const arrayBuffer = atob(imageURL.split(',')[1]);
		const uint8Array = new Uint8Array(arrayBuffer.length);

		for (let i = 0; i < arrayBuffer.length; i++) {
			uint8Array[i] = arrayBuffer.charCodeAt(i);
		}

		return new Blob([uint8Array], { type: 'image/png' });
	};

	const shareMeme = () => {
		if (!navigator.share) {
			alert('Your browser does not support the Web Share API');
			return;
		}

		const memeImage = getImage();

		try {
			navigator.share({
				title: 'Check out this meme!',
				text: 'Here is a funny meme I generated with CapMeme.',
				url: 'https://capmeme.pro',
				files: [new File([memeImage], 'meme.png', { type: 'image/png' })]
			});
		} catch (error) {
			console.error('Error sharing meme:', error);
		}
	};
</script>

<button class="group" on:click={shareMeme}>
	<svg aria-hidden="true" class="mr-4 h-6 w-6 fill-white/50 group-hover:fill-white/70">
		<path
			d="M10.1141,4.49112 L9.91063,7.63542 L9.891,8.05196 L9.8012,8.06134 C5.36297,8.583 2,12.3671 2,17 C2,17.457 2.03414,17.91 2.10168,18.3565 C2.38094,20.2022 2.59088,20.3807 3.87391,18.8547 C4.18977,18.479 4.54227,18.1439 4.91368,17.8247 C6.24977,16.7224 7.90632,16.0786 9.66842,16.0067 L9.894,16.002 L9.95549,17.2308 L10.1215,19.576 C10.2008,20.38 11.0467,20.9293 11.8253,20.4902 C12.1766,20.2919 12.52,20.0809 12.8641,19.8706 C14.652,18.7519 16.3249,17.4666 17.9553,16.1321 C18.9147,15.3326 19.7558,14.5744 20.4714,13.8844 C20.8007,13.5606 21.1304,13.2376 21.4496,12.9037 C21.9118,12.42 21.9575,11.6189 21.4737,11.1124 C20.3603,9.94706 18.7862,8.48751 16.8271,6.94049 C15.2394,5.69825 13.597,4.53773 11.8571,3.51856 C11.0203,3.04172 10.1902,3.69599 10.1141,4.49112 Z"
		>
		</path>
	</svg>
</button>
