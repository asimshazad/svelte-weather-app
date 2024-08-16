<script>
    let city = '';
    let weather = null;
    let error = null;
  
    async function fetchWeather() {
      if (city.trim() === '') return;
      try {
        const response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${import.meta.env.VITE_OPENWEATHER_API_KEY}`
        );

        if (!response.ok) throw new Error('City not found');
        weather = await response.json();
        error = null;
      } catch (err) {
        weather = null;
        error = err.message;
      }
    }
  </script>
  
  <div class="max-w-md mx-auto mt-10 p-6 bg-white shadow-md rounded-lg">
    <h1 class="text-2xl font-bold text-center mb-6">Svelte Weather App</h1>
  
    <div class="mb-4">
      <input
        type="text"
        class="border w-full px-3 py-2 rounded-md focus:outline-none"
        bind:value={city}
        placeholder="Enter city name"
      />
      <button
        class="mt-2 w-full bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600"
        on:click={fetchWeather}
      >
        Get Weather
      </button>
    </div>
  
    {#if weather}
      <div class="text-center">
        <h2 class="text-xl font-bold">{weather.name}, {weather.sys.country}</h2>
        <p class="text-lg">Temperature: {weather.main.temp}°C</p>
        <p class="text-lg">Weather: {weather.weather[0].description}</p>
        <p>Humidity: {weather.main.humidity}%</p>
        <p>Wind Speed: {weather.wind.speed} m/s</p>
        <p>Pressure: {weather.main.pressure} hPa</p>
        <img
          src={`http://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`}
          alt={weather.weather[0].description}
        />

      </div>
    {/if}
  
    {#if error}
      <p class="text-red-500 text-center mt-4">{error}</p>
    {/if}
  </div>
  