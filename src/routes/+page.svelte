<script>
	let latitude = null;
	let longitude = null;
	let accuracy = null;
	let errorMessage = null;

	function getLocation() {
		if (navigator.geolocation) {
			navigator.geolocation.getCurrentPosition(showPosition, showError);
		} else {
			errorMessage = "Geolocation wird von diesem Browser nicht unterstützt.";
		}
	}

	function showPosition(position) {
		latitude = position.coords.latitude;
		longitude = position.coords.longitude;
		accuracy = position.coords.accuracy;
		errorMessage = null;
	}

	function showError(error) {
		errorMessage = "Es gibt einen Fehler beim Abrufen des Standorts.";
	}
</script>

<div class="flex items-center justify-center min-h-screen bg-gradient-to-br from-blue-900 via-blue-700 to-green-600 text-white">
	<div class="bg-white-10  rounded-2xl text-center max-w-md w-full">
		<h1 class="text-3xl font-bold mb-6 text-blue-200"> Geolocation</h1>
		
		<button 
			on:click={getLocation}
			class="px-6 py-2 rounded-xl bg-blue-500"
		>
			Standort zeigen
		</button>

		{#if errorMessage}
			<p class="text-red-300 mb-4">{errorMessage}</p>
		{/if}

		<ul class="space-y-2 text-left text-blue-100">
			<li><strong>Breitengrad:</strong> {latitude}</li>
			<li><strong>Längengrad:</strong> {longitude}</li>
			<li><strong>Genauigkeit (m):</strong> {accuracy}</li>
		</ul>
	</div>
</div>
