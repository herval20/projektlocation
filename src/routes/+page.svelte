<script>
  let latitude = null;
  let longitude = null;
  let country = null;
  let errorMessage = null;

  async function getLocation() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(showPosition, showError);
    } else {
      errorMessage = "Geolocation wird nicht unterstützt.";
    }
  }

  async function showPosition(position) {
    latitude = position.coords.latitude;
    longitude = position.coords.longitude;

    try {
      const res = await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}`);
      const data = await res.json();
      console.log("JSON:", data);

      country = data.address?.country || "Land konnte nicht ermittelt werden";
    } catch (err) {
      country = "Fehler bei der Abfrage";
    }
  }

  function showError(error) {
    errorMessage = error.message;
  }
</script>

<div class="flex flex-col items-center justify-center min-h-screen bg-gradient-to-br from-blue-900 via-blue-700 to-green-600 p-6">
  <div class="bg-white/100 p-8 rounded-2xl shadow-xl text-center max-w-md w-full">
    <h1 class="text-3xl font-bold mb-6 text-blue-200">Geolocation</h1>

    <button 
      on:click={getLocation}
      class="px-6 py-2 rounded-xl bg-blue-500 hover:bg-blue-600 transition-colors font-semibold text-white shadow-lg mb-6"
    >
      Standort abrufen
    </button>

    {#if errorMessage}
      <p class="text-red-300 mb-4">{errorMessage}</p>
    {/if}

    {#if latitude && longitude}
      <div class="mt-6 space-y-2">
        <p class="text-blue-500"><span class="font-semibold">Latitude:</span> {latitude}</p>
        <p class="text-blue-500"><span class="font-semibold">Longitude:</span> {longitude}</p>
        <p class="text-blue-500"><span class="font-semibold">Land:</span> {country}</p>
      </div>
    {/if}
  </div>
</div>
