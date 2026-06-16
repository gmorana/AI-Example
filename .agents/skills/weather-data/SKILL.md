---
name: weather-data
description: "Fetch and integrate real-time weather data into web applications. Use when adding weather displays, building weather dashboards, integrating forecasts, or handling API authentication and parsing weather responses."
argument-hint: "API provider (openweathermap, weatherapi, tomorrow.io) and use case"
---

# Weather Data Integration

Streamlined workflow for fetching weather data from public APIs and integrating it into web pages or applications.

## When to Use

- Building weather display components for web pages
- Adding real-time weather forecasts to applications
- Creating weather dashboards or widgets
- Integrating weather data into existing projects
- Testing weather API integrations
- Debugging API responses and data parsing

## Weather API Comparison

| Provider           | Free Tier              | Key Features                                | Best For                    |
| ------------------ | ---------------------- | ------------------------------------------- | --------------------------- |
| **OpenWeatherMap** | 5 calls/min, 1,000/day | Simple API, extensive docs                  | Quick prototypes, learning  |
| **WeatherAPI**     | 1M calls/month         | Detailed forecast, no key required for demo | Production apps, forecasts  |
| **Tomorrow.io**    | 500 calls/day          | ML-powered, precise                         | Weather-critical apps       |
| **Open-Meteo**     | Unlimited free         | No key needed, CORS-friendly                | Serverless, privacy-focused |

## Setup Procedure

### 1. Choose Your API

Select based on:

- **Free tier limits** (calls per day/month)
- **Data availability** (current weather, forecast, historical)
- **Geographic coverage** (global vs regional)
- **Response format** (JSON structure and fields)

**Recommendation for beginners**: OpenWeatherMap or Open-Meteo (no auth required for Open-Meteo)

### 2. Obtain API Credentials

#### OpenWeatherMap

1. Go to [openweathermap.org](https://openweathermap.org/api)
2. Sign up for free account
3. Navigate to API keys section
4. Copy default API key
5. Store in `.env` file: `OPENWEATHER_API_KEY=your_key_here`

#### WeatherAPI

1. Visit [weatherapi.com](https://www.weatherapi.com/)
2. Sign up for free tier
3. API key automatically generated in dashboard
4. Store: `WEATHER_API_KEY=your_key_here`

#### Open-Meteo

- No authentication required
- Use directly: `https://api.open-meteo.com/v1/forecast`

### 3. Fetch Weather Data

#### Using Fetch API (JavaScript)

```javascript
// OpenWeatherMap example
const apiKey = process.env.OPENWEATHER_API_KEY;
const lat = 40.7128;
const lon = -74.006;

fetch(
  `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}&units=metric`,
)
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.error("Weather API Error:", error));
```

#### Using Open-Meteo (No Auth)

```javascript
const lat = 40.7128;
const lon = -74.006;

fetch(
  `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current=temperature_2m,weather_code`,
)
  .then((response) => response.json())
  .then((data) => console.log(data.current))
  .catch((error) => console.error("Error:", error));
```

### 4. Parse Response Data

Extract relevant fields from API response:

```javascript
// Typical weather response structure
const weather = {
  temperature: data.current?.temp || data.main?.temp,
  condition: data.current?.weather?.[0]?.main || data.weather?.[0]?.main,
  humidity: data.main?.humidity,
  windSpeed: data.wind?.speed,
  feelsLike: data.main?.feels_like,
  pressure: data.main?.pressure,
};
```

### 5. Handle Errors and Edge Cases

- **Rate limiting**: Implement exponential backoff retry logic
- **Timeout**: Set 5-10 second timeout on fetch requests
- **Invalid coordinates**: Validate latitude (-90 to 90) and longitude (-180 to 180)
- **Missing API key**: Fail gracefully with user-friendly message
- **Network errors**: Catch and log errors; display cached data if available

```javascript
async function getWeatherWithRetry(lat, lon, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}`,
        { signal: AbortSignal.timeout(5000) },
      );
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await new Promise((r) => setTimeout(r, Math.pow(2, i) * 1000));
    }
  }
}
```

### 6. Integrate into Web Page

#### HTML Structure

```html
<div id="weather-widget">
  <h2 id="location">Loading...</h2>
  <div id="temperature">--°C</div>
  <p id="condition">--</p>
  <p id="details">
    <span>Humidity: <span id="humidity">--%</span></span> |
    <span>Wind: <span id="wind">-- m/s</span></span>
  </p>
</div>
```

#### JavaScript Integration

```javascript
async function displayWeather(lat, lon) {
  try {
    const data = await getWeatherWithRetry(lat, lon);

    document.getElementById("location").textContent = data.name;
    document.getElementById("temperature").textContent =
      `${Math.round(data.main.temp)}°C`;
    document.getElementById("condition").textContent = data.weather[0].main;
    document.getElementById("humidity").textContent = `${data.main.humidity}%`;
    document.getElementById("wind").textContent = `${data.wind.speed} m/s`;
  } catch (error) {
    document.getElementById("weather-widget").textContent =
      `Error: ${error.message}`;
  }
}

// Fetch weather for New York
displayWeather(40.7128, -74.006);
```

### 7. Environment & Deployment

**For local development:**

1. Create `.env` file in project root
2. Add `OPENWEATHER_API_KEY=your_key`
3. Use tool like `dotenv` to load environment variables

**For production:**

- Store API keys in environment variables (not in code)
- Use API gateway or backend proxy to hide keys from client
- Implement rate limiting and caching
- Consider CDN or edge caching for weather data

## Testing Checklist

- [ ] API key is valid and active
- [ ] Coordinates are within valid ranges
- [ ] Response data parsed correctly
- [ ] Error handling works for network failures
- [ ] UI updates with fresh data
- [ ] Rate limits respected (cache if needed)
- [ ] Works with different geographic locations
- [ ] Graceful degradation if API unavailable

## Common Issues & Solutions

| Issue                 | Solution                                            |
| --------------------- | --------------------------------------------------- |
| 401 Unauthorized      | Verify API key is correct and not expired           |
| 429 Too Many Requests | Reduce call frequency or upgrade plan               |
| CORS errors           | Use backend proxy or CORS-friendly API (Open-Meteo) |
| Missing fields        | Check API response format for your provider         |
| Stale data            | Implement cache with TTL (Time To Live)             |

## Next Steps

1. Choose an API provider and get credentials
2. Test fetching data in browser console
3. Parse response and display in UI
4. Add error handling and retry logic
5. Cache responses to reduce API calls
6. Deploy with secure credential storage

## Related Customizations

- Create instructions for weather widget component patterns
- Build a custom agent for weather-specific queries
- Set up hooks for automated weather data refresh
