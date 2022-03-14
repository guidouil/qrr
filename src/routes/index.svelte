<script>
	import '../app.css';
	import QRCode from 'qrcode';

	import NavBar from '$lib/NavBar.svelte';
	let targetUrl = 'https://qrr.re';
	let qrSvg = '';
	const makeQrCode = () => {
		console.log(targetUrl);
		if (targetUrl) {
			const options = {
				type: 'svg',
				errorCorrectionLevel: 'L',
				color: {
					dark: '#ffffff',
					light: '#000000'
				}
			};
			QRCode.toString(targetUrl, options, (err, qrString) => {
				if (err) console.error(err);
				if (qrString) qrSvg = qrString;
			});
		}
	};
	makeQrCode();
</script>

<svelte:head>
	<title>qrr.re</title>
</svelte:head>

<main class="mx-auto">
	<NavBar />
	<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
		<div class="max-w-lg mx-auto">
			<h1 class="text-2xl">Les QR Codes les plus rapides de l'ouest</h1>
			<em>Les QR codes sur fond noir sont plus rapide à scanner.</em>
			<div class="divider" />
			<label class="label" for="targetUrl">Adresse URL pour le qr-code</label>
			<input
				class="input input-bordered input-primary input-lg w-full"
				type="url"
				name="targetUrl"
				id="targetUrl"
				bind:value={targetUrl}
				on:input={() => makeQrCode()}
			/>
		</div>
		<div class="max-w-2xl">
			{@html qrSvg}
		</div>
	</div>
</main>
