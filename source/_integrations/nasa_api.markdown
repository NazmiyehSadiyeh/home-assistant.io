---
title: "NASA API"
description: "Integrate NASA's public APIs for astronomy, asteroids, and Mars weather data."
ha_version: "2024.1"
logo: /images/integrations/nasa_api/logo.png
---

# NASA API

## Overview

The NASA API integration connects Home Assistant with NASA's publicly available APIs, enabling users to access:
- Stunning daily astronomy images and explanations from the **Astronomy Picture of the Day (APOD)** API.
- Near-Earth Object data, such as asteroid size, velocity, and orbit information, from the **Asteroids - NeoWs** API.
- Daily Mars weather updates from the **InSight: Mars Weather Service API**.

This integration is designed to bring NASA's scientific and astronomical data directly into your smart home, allowing you to use this data in automations, notifications, or dashboards.

### Idea Behind the Integration
The idea behind the NASA API integration is to combine the wealth of information provided by NASA with the power of Home Assistant. Whether you're an astronomy enthusiast, curious about asteroids, or fascinated by Mars, this integration provides a seamless way to explore and utilize NASA's data in your daily life.

---

## NASA APIs Overview

### 1. Astronomy Picture of the Day (APOD)
- **Description:** Provides a daily image or video along with a description written by professional astronomers.
- **Use Cases:**
  - Display the image on your dashboard.
  - Send the image as part of a daily notification.
- **Endpoint:** `/planetary/apod`

### 2. Asteroids - NeoWs
- **Description:** Tracks near-Earth objects (NEOs), providing data on their orbits, size, velocity, and proximity to Earth.
- **Use Cases:**
  - Monitor asteroids passing near Earth.
  - Trigger automations based on asteroid activity.
- **Endpoint:** `/neo/rest/v1/feed`

### 3. InSight: Mars Weather Service
- **Description:** Fetches daily weather reports from NASA's InSight mission, which monitors temperature, wind speed, and pressure on Mars.
- **Use Cases:**
  - Display Mars weather on a dedicated dashboard.
  - Use Mars weather data for educational purposes or as a fun automation trigger.
- **Endpoint:** `/insight_weather/`

---

## Implementation Details

### 1. Astronomy Picture of the Day (APOD)
The APOD API fetches the astronomy picture of the day along with metadata like title and explanation.

#### Configuration
Add the following YAML configuration to enable APOD:
```yaml
nasa_api:
  api_key: YOUR_API_KEY
  apis:
    - apod