![Nominatim API Featured Image](https://raw.githubusercontent.com/omkarcloud/nominatim-api/master/nominatim-api-featured-image.png)

# Nominatim API

Geocode any city name to coordinates, bounding box, and full GeoJSON boundary polygon via a simple REST API. Get the exact shape of any city on Earth — not just a pin on a map. 100 free requests/month.

## Key Features

- Geocode city names to center coordinates, bounding box, and full GeoJSON boundary polygon
- Get exact city boundary shapes — draw any city outline on a map with one API call
- Disambiguate with optional state and country code parameters
- Worldwide coverage — every city OpenStreetMap has data for
- No rate limit headaches — unlike the official Nominatim API (1 req/sec)
- **100 requests/month on free tier**
- Example Response:
```json
{
  "place_id": 419365556,
  "osm_type": "relation",
  "osm_id": 207359,
  "addresstype": "city",
  "name": "Los Angeles",
  "display_name": "Los Angeles, Los Angeles County, California, United States",
  "boundingbox": ["33.6595410", "34.3373060", "-118.6681798", "-118.1552983"],
  "geojson": {
    "type": "Polygon",
    "coordinates": [[[-118.6681798, 34.1849312], "..."]]
  },
  "center_point": {
    "latitude": 34.0536909,
    "longitude": -118.242766
  }
}
```

## Get API Key

Create an account at [omkar.cloud](https://www.omkar.cloud/auth/sign-up?redirect=/api-key) to get your API key.

It takes just 2 minutes to sign up. You get 100 free requests every month for city geocoding.

This is a well built product, and your search for the best Geocoding API with GeoJSON boundaries ends right here.

## Quick Start

```bash
curl -X GET "https://nominatim-api.omkar.cloud/nominatim/geocode?city=San%20Francisco&state=California&country_code=us" \
  -H "API-Key: YOUR_API_KEY"
```

```json
{
  "place_id": 419365556,
  "osm_type": "relation",
  "osm_id": 207359,
  "addresstype": "city",
  "name": "Los Angeles",
  "display_name": "Los Angeles, Los Angeles County, California, United States",
  "boundingbox": ["33.6595410", "34.3373060", "-118.6681798", "-118.1552983"],
  "geojson": {
    "type": "Polygon",
    "coordinates": [[[-118.6681798, 34.1849312], [-118.6681702, 34.1767647], "..."]]
  },
  "center_point": {
    "latitude": 34.0536909,
    "longitude": -118.242766
  }
}
```

## Quick Start (Python)

```bash
pip install requests
```

```python
import requests

response = requests.get(
    "https://nominatim-api.omkar.cloud/nominatim/geocode",
    params={"city": "San Francisco", "state": "California", "country_code": "us"},
    headers={"API-Key": "YOUR_API_KEY"}
)

print(response.json())
```


## API Reference

### City Geocode

```
GET https://nominatim-api.omkar.cloud/nominatim/geocode
```

#### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `city` | Yes | — | City name to geocode. |
| `state` | No | — | State or province name for disambiguation. |
| `country_code` | No | — | ISO 3166-1 alpha-2 country code (e.g., `us`, `jp`, `gb`). |

#### Example

```python
import requests

response = requests.get(
    "https://nominatim-api.omkar.cloud/nominatim/geocode",
    params={"city": "Los Angeles"},
    headers={"API-Key": "YOUR_API_KEY"}
)

print(response.json())
```

#### Response

<details>
<summary>Sample Response (click to expand)</summary>

```json
{
  "place_id": 419365556,
  "licence": "Data © OpenStreetMap contributors, ODbL 1.0. http://osm.org/copyright",
  "osm_type": "relation",
  "osm_id": 207359,
  "class": "boundary",
  "type": "administrative",
  "place_rank": 16,
  "importance": 0.8410828881088647,
  "addresstype": "city",
  "name": "Los Angeles",
  "display_name": "Los Angeles, Los Angeles County, California, United States",
  "boundingbox": [
    "33.6595410",
    "34.3373060",
    "-118.6681798",
    "-118.1552983"
  ],
  "geojson": {
    "type": "Polygon",
    "coordinates": [
      [
        [-118.6681798, 34.1849312],
        [-118.6681702, 34.1767647],
        [-118.6586162, 34.1767595],
        [-118.6586436, 34.1694539],
        [-118.6681798, 34.1849312]
      ]
    ]
  },
  "center_point": {
    "latitude": 34.0536909,
    "longitude": -118.242766
  }
}
```

</details>

## Error Handling

```python
response = requests.get(
    "https://nominatim-api.omkar.cloud/nominatim/geocode",
    params={"city": "San Francisco"},
    headers={"API-Key": "YOUR_API_KEY"}
)

if response.status_code == 200:
    data = response.json()
elif response.status_code == 401:
    # Invalid API key
    pass
elif response.status_code == 404:
    # City not found
    pass
elif response.status_code == 429:
    # Rate limit exceeded
    pass
```

## FAQs

### What data does the API return?

**City Geocode** returns per city:
- Place ID, OSM type, and OSM ID
- Address type and place rank
- City name and full display name (city, county, state, country)
- Bounding box (south, north, west, east coordinates)
- Full GeoJSON boundary polygon with exact city boundaries
- Center point coordinates (latitude, longitude)
- Importance score from OpenStreetMap

All in structured JSON. Ready to plot on a map or use in your app.

### How accurate is the data?

Data is sourced from OpenStreetMap via Nominatim. City boundaries, coordinates, and bounding boxes reflect the latest OpenStreetMap data. OSM is maintained by a global community of mappers and is the same data source used by Apple Maps, Facebook, and thousands of other services.

### Why should I use this instead of the official Nominatim API?

The official Nominatim API enforces a strict rate limit of 1 request per second. That's fine for a quick test, but it becomes a bottleneck fast when you're building a real product, with unpredictable usage demands.

With our API, you get high-throughput access without worrying about rate limits.

### What's the difference between center point, bounding box, and GeoJSON polygon?

**Center point** gives you a single latitude/longitude coordinate for the city center. Good for placing a marker on a map.

**Bounding box** gives you the rectangular extent of the city — south, north, west, east edges. Good for setting map zoom levels or filtering locations within a region.

**GeoJSON polygon** gives you the actual city boundary shape. Good for geofencing, delivery zones, or drawing the exact city outline on a map.

### What if there are multiple cities with the same name?

Use `state` and `country_code` to disambiguate. For example, "Portland" exists in both Oregon State and Maine State.

- `city=Portland&state=Oregon&country_code=us` — gets Portland, Oregon State, US
- `city=Portland&state=Maine&country_code=us` — gets Portland, Maine State, US

Without disambiguation, the API returns the most prominent result.

### Can I geocode cities from any country?

Yes. Pass any city name and optionally a `country_code` (2-letter ISO code) to disambiguate. Works worldwide — US cities, European cities, Asian cities, everywhere OpenStreetMap has data. That's essentially every city on Earth.

## Rate Limits

| Plan | Price | Requests/Month |
|------|-------|----------------|
| Free | $0 | 100 |
| Starter | $16 | 3,000 |
| Grow | $48 | 15,000 |
| Scale | $148 | 75,000 |

## Questions? We have answers.

Reach out anytime. We will solve your query within 1 working day.

[![Contact Us on WhatsApp about Nominatim API](https://raw.githubusercontent.com/omkarcloud/assets/master/images/whatsapp-us.png)](https://api.whatsapp.com/send?phone=918178804274&text=I%20have%20a%20question%20about%20the%20Nominatim%20API.)

[![Contact Us on Email about Nominatim API](https://raw.githubusercontent.com/omkarcloud/assets/master/images/ask-on-email.png)](mailto:happy.to.help@omkar.cloud?subject=Nominatim%20API%20Question)
