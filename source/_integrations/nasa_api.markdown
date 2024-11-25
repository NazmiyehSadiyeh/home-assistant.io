---
title: "NASA API"
description: "Integrate NASA's public APIs for astronomy, asteroids, and Mars weather data."
ha_version: "2024.1"
ha_category: "Weather"
ha_iot_class: "Cloud Polling"
ha_quality_scale: silver
ha_config_flow: true
ha_codeowners:
  - '@NazmiyehSadiyeh'
ha_domain: nasa_api
---

## Overview

The **NASA API Integration** for Home Assistant brings space exploration closer to your fingertips by enabling data from NASA's public APIs. Users can display space-related imagery, track near-Earth asteroids, and monitor Martian weather conditions directly in Home Assistant.

This integration supports the following APIs:

- **APOD (Astronomy Picture of the Day)**: Showcases daily astronomy-related imagery or video.
- **Asteroids - NeoWs (Near Earth Object Web Service)**: Tracks near-Earth objects (NEOs), including their sizes, velocities, and hazard potential.
- **InSight: Mars Weather Service**: Displays real-time Martian weather data from NASA's InSight lander.

---

## Features

| API                | Description                                                                                 | Entities/Features                                                                                                                                          |
|--------------------|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **APOD**           | Displays NASA's Astronomy Picture of the Day.                                               | - Entity for the APOD image or video<br>- Metadata: Title, description, and date                                                                           |
| **Asteroids - NeoWs** | Tracks near-Earth objects (NEOs), including orbital details and hazardous asteroids.        | - Entities for closest approach distance, smallest and largest NEO diameters, average velocity, and total count                                           |
| **InSight: Mars Weather** | Provides weather data from NASA's InSight lander on Mars.                               | - Entities for temperature (min/max/average), atmospheric pressure, and wind speed                                                                        |

---

## Configuration

The NASA API integration is configured through the Home Assistant UI:

1. Navigate to **Settings > Integrations**.
2. Search for **NASA API** and click to add it.
3. Select the data sources you want to enable. A checklist will appear with the three available APIs: 
   - **APOD (Astronomy Picture of the Day)**
   - **Asteroids - NeoWs (Near Earth Object Web Service)**
   - **InSight: Mars Weather Service**
   You can choose one, two, or all of them based on your needs.
4. Enter your NASA API key. You can use the provided **DEMO_KEY**, but it is recommended to generate your own API key for full access and to avoid rate limits. API keys can be obtained for free from the [NASA Developer Portal](https://api.nasa.gov/).
5. Save the configuration.

---

## Data Sources

### **Astronomy Picture of the Day (APOD)**

<img src="../../images/integrations/nasa_api/APOD.png" alt="APOD Image" width="500" />

*Example of an Astronomy Picture of the Day displayed in the Home Assistant dashboard.*

#### Purpose
NASA's APOD provides a daily image or video with an accompanying description, offering insights into astronomy and space exploration.

#### How It's Displayed
- **Entity**: `sensor.apod_image`
- **Attributes**:
  - `title`: The image or video's title.
  - `description`: A detailed explanation.
  - `date`: The date the APOD was featured.

#### Example in Home Assistant
The APOD image or video is displayed in the dashboard, giving users a visual highlight of the day.

#### Example Data
```json
{
  "date": "2024-11-22",
  "title": "Earthrise Beyond the Moon",
  "url": "https://apod.nasa.gov/apod/image/earthrise.jpg",
  "explanation": "A historic capture of Earth rising over the lunar horizon."
}
```

### **Asteroids - NeoWs (Near Earth Object Web Service)**

<img src="../../images/integrations/nasa_api/NEO.png" alt="NEOs Image" width="500" />

*Example of Near Earth Objects data visualization, including closest approach distances and hazardous asteroid counts.*

Purpose

NeoWs tracks near-Earth objects, offering vital details such as:
	•	Sizes
	•	Velocities
	•	Closest and farthest approach distances
	•	Hazardous asteroid counts

How It’s Displayed

	•	Entities:
	•	sensor.closest_approach_distance
	•	sensor.largest_neo_diameter
	•	sensor.total_neo_count
	•	Attributes: Include asteroid velocities, distances, and counts.

Example in Home Assistant

Near-Earth Object data is summarized, as shown in this screenshot:

Example Data
```json
{
  "element_count": 24,
  "near_earth_objects": [
    {
      "name": "2024 XYZ",
      "close_approach_date": "2024-11-23",
      "miss_distance_km": "250000.12",
      "estimated_diameter_km": {
        "min": 0.05,
        "max": 0.1
      },
      "is_potentially_hazardous_asteroid": true
    }
  ]
}
```

InSight: Mars Weather Service

Purpose

Provides daily Martian weather data gathered from NASA’s InSight lander.

How It’s Displayed

	•	Entities:
	•	sensor.mars_temperature (min/max/average)
	•	sensor.mars_pressure (atmospheric pressure)
	•	sensor.mars_wind_speed (wind speed)
	•	These sensors display real-time data about the Martian climate.

Example in Home Assistant

Mars weather is visualized in widgets such as:

Example Data
```json
{
  "sol_keys": ["1000"],
  "1000": {
    "AT": {"av": -60.4, "mn": -80.1, "mx": -40.5},
    "PRE": {"av": 865.1},
    "HWS": {"av": 5.3}
  }
}
```

Example YAML Configuration

Advanced users may use YAML to configure the integration:

nasa_api:
  api_key: "your_nasa_api_key"
  data_sources:
    - apod
    - asteroids
    - mars_weather

Troubleshooting

	•	Cannot connect to NASA API:
	•	Ensure your API key is correct and active.
	•	Verify your network connection.
	•	No data displayed:
	•	Check if entities are properly created and visible in Home Assistant.
	•	Ensure you’ve enabled the APIs in the configuration.
	•	Rate limiting:
	•	NASA’s free API key has limits. If you hit them, request an upgraded API key from NASA.

Additional Notes

	•	API data availability is subject to NASA’s uptime and performance.
	•	Update intervals for sensors can be modified in coordinator.py.



