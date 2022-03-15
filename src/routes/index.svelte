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

	const downloadQrCode = () => {
		const svgBlob = new Blob([qrSvg], { type: 'image/svg+xml;charset=utf-8' });
		const svgUrl = URL.createObjectURL(svgBlob);
		const downloadLink = document.createElement('a');
		downloadLink.href = svgUrl;
		downloadLink.download = `QR_Code_${targetUrl.split('//')[1]}.svg`;
		document.body.appendChild(downloadLink);
		downloadLink.click();
		document.body.removeChild(downloadLink);
	};
</script>

<svelte:head>
	<title>qrr.re</title>
</svelte:head>

<main class="mx-auto">
	<NavBar />
	<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
		<div class="max-w-lg mx-auto px-8">
			<h1 class="text-2xl">Les QR Codes les plus rapides de l'ouest</h1>
			<small><em>Les QR Codes sur fond noir sont plus rapide à scanner.</em></small>
			<div class="divider" />
			<form>
				<label class="label" for="targetUrl">Adresse URL pour le QR Code</label>
				<input
					class="input input-bordered input-primary input-lg w-full"
					type="url"
					name="targetUrl"
					id="targetUrl"
					bind:value={targetUrl}
					on:input={() => makeQrCode()}
				/>
			</form>
			<div class="divider" />
			<button class="btn btn-primary btn-block" on:click={downloadQrCode}>
				Télécharger le QR Code
			</button>
		</div>
		<div class="max-w-2xl">
			{@html qrSvg}
		</div>
	</div>
</main>
