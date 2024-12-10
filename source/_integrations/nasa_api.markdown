---
title: "NASA API"
description: "Integrate NASA's public APIs for astronomy, asteroids, and Mars weather data."
ha_version: "2024.1"
ha_release: "2024.12"
ha_category: "Weather"
ha_iot_class: "Cloud Polling"
ha_quality_scale: silver
ha_config_flow: true
ha_codeowners:
  - '@NazmiyehSadiyeh'
ha_domain: nasa_api
---

## Overview

The **NASA API Integration** for Home Assistant brings space exploration closer to your fingertips by enabling data from NASA's public APIs. Users can display space-related imagery, track near-Earth asteroids and monitor Martian weather conditions directly in Home Assistant.

This integration supports the following APIs:

- **APOD (Astronomy Picture of the Day)**: Showcases daily astronomy-related imagery.
- **Asteroids - NeoWs (Near Earth Object Web Service)**: Tracks near-Earth objects (NEOs), including their sizes, velocities, and hazard potential.
- **InSight: Mars Weather Service**: Displays real-time Martian weather data from NASA's InSight lander.

---

## Features

| API                | Description                                                                                 | Entities/Features                                                                                                                                          |
|--------------------|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **APOD**           | Displays NASA's Astronomy Picture of the Day.                                               | - Entity for the APOD image<br>- Metadata: Title, description, and date                                                                           |
| **Asteroids - NeoWs** | Tracks near-Earth objects (NEOs), including orbital details and hazardous asteroids.        | - Entities for closest approach distance, smallest and largest NEO diameters, average velocity, and total count                                           |
| **InSight: Mars Weather** | Provides weather data from NASA's InSight lander on Mars.                               | - Entities for temperature (min/max/average), atmospheric pressure, and wind speed                                                                        |

---

## Configuration

The NASA API integration is configured through the Home Assistant UI:

1. Navigate to **Settings > Integrations**.
2. Search for **NASA API** and click to add it.
3. Enter your NASA API key. You can use the provided **DEMO_KEY**, but it is recommended to generate your own API key for full access and to avoid rate limits. API keys can be obtained for free from the [NASA Developer Portal](https://api.nasa.gov/).
4. Select the data sources you want to enable. A checklist will appear with the three available APIs:
   - **APOD (Astronomy Picture of the Day)**
   - **Asteroids - NeoWs (Near Earth Object Web Service)**
   - **InSight: Mars Weather Service**
5. If you choose **NeoWs** in your selection, another pop-up will appear asking you to choose a room in your home for the integration. You don't need to select a room. You can simply press **Finish** to complete the configuration.
6. Save the configuration.

---

## Data Sources

### **Astronomy Picture of the Day (APOD)**

The APOD integration uses the **ImageEntity** from Home Assistant. This entity fetches an image from NASA’s APOD API and displays it within Home Assistant. The image URL is provided through this entity, and additional metadata like the title and description are available as attributes.

- **ImageEntity** is designed for handling images and is optimized for displaying large image files. The entity fetches the image and makes it accessible within Home Assistant’s dashboard.

#### Purpose

NASA's APOD provides a daily image with an accompanying description, offering insights into astronomy and space exploration.

#### How It's Displayed

- **Entity**: `image.astronomy_picture_of_the_day`
- **Attributes**:
  - `title`: The image's title.
  - `description`: A detailed explanation.
  - `date`: The date the APOD was featured.


<img src="../../images/integrations/nasa_api/APOD.png" alt="APOD Image" width="500" />

*Example of an Astronomy Picture of the Day displayed in the Home Assistant dashboard.*

#### Example Data

```json
{
  "date": "2024-11-22",
  "title": "Earthrise Beyond the Moon",
  "url": "https://apod.nasa.gov/apod/image/earthrise.jpg",
  "explanation": "A historic capture of Earth rising over the lunar horizon."
}
```

#### Limitations of APOD Display

Due to the limitations of the image entity in Home Assistant, clicking on the APOD image on the dashboard will not display additional details such as the title or explanation. These details are still fetched and available within Home Assistant, but they are not visible on the dashboard by default. This behavior may change with future updates or custom UI configurations.


### **Asteroids - NeoWs (Near Earth Object Web Service)**

The NEOs integration uses the **SensorEntity** from Home Assistant. This entity tracks various statistics about near-Earth objects (NEOs) provided by NASA’s NeoWs API. It includes data such as the number of NEOs, their sizes, velocities, and hazard potential. These values are displayed in the Home Assistant dashboard as individual sensor entities.

- **SensorEntity** is used to represent numeric or text-based values and is ideal for displaying real-time data such as measurements or counts. The entity fetches the latest data about NEOs and makes it available as individual sensors in the Home Assistant interface.

#### Purpose

NASA’s NeoWs API tracks near-Earth objects, providing crucial information such as asteroid sizes, velocities, closest approach distances, and whether any of them are potentially hazardous. This data helps raise awareness of the objects that come close to Earth’s orbit.

#### How It’s Displayed

**Card View:**

<img src="../../images/integrations/nasa_api/NEO.png" alt="NEOs Image" width="500" />

*Example of Near Earth Objects data visualization, including closest approach distances and hazardous asteroid counts.*

**Expanded View:**

When you click on any of the summary statistics in the NEO card (e.g., Average NEO Diameter, Total NEO Count), it will expand to show more detailed information, including a historical graph. The example below shows the history of the Average NEO Diameter visualized over time. Giving users a better understanding of the changes in the data.

<img src="../../images/integrations/nasa_api/NEO_click.png" alt="NEOs Expanded View" width="500" />

*Clicking on a summary statistic in the NEO card reveals the historical data, such as the graph for the Average NEO Diameter.*

#### Entities

- sensor.closest_approach_distance
- sensor.largest_neo_diameter
- sensor.total_neo_count
- Attributes: Include asteroid velocities, distances, and counts.

#### Example Data

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

### InSight: Mars Weather Service

The InSight Mars Weather integration uses the **WeatherEntity** from Home Assistant. This entity fetches real-time weather data from NASA’s InSight Mars lander API. It provides details about Mars’ temperature, atmospheric pressure, wind speed, and season. These values are made available in Home Assistant’s dashboard as a weather entity.

- **WeatherEntity** is specifically designed for displaying weather-related data. It provides attributes like temperature, pressure, wind speed, and conditions, making it ideal for tracking weather information from both Earth and other planets, like Mars in this case.

#### Purpose

NASA’s InSight Mars Weather API provides daily weather updates from Mars, offering insights into the planet’s climate. This includes detailed measurements of temperature (min/max/average), atmospheric pressure, wind speed and seasonal changes.

#### How It’s Displayed

**Card View**  

<img src="../../images/integrations/nasa_api/Mars_1.png" alt="Mars Weather Card View" width="500" />

*Example of Mars Weather in the card view, showing the current temperature and weather condition.*

**Expanded View**  
<img src="../../images/integrations/nasa_api/Mars_2.png" alt="Mars Weather Expanded View" width="500" />

*Example of Mars Weather in the expanded view, showing more detailed weather information like pressure and wind speed.*

#### Entities:

- sensor.mars_temperature (min/max/average)
- sensor.mars_pressure (atmospheric pressure)
- sensor.mars_wind_speed (wind speed)
- These sensors display real-time data about the Martian climate.

#### Example in Home Assistant

Mars weather is visualized in widgets such as:

#### Example Data

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

---

## Custom Dashboard for the NASA integration

To enhance the user experience and overcome the limitations of the APOD integration, where clicking on the image doesn’t provide additional details, creating a custom dashboard in Home Assistant is recommended. 
This dashboard will display detailed information for all available NASA data sources, offering richer insights and a more interactive experience.

- **APOD (Astronomy Picture of the Day):** The custom dashboard displays the APOD integration with its title, description and image. This provides a complete view of the daily astronomy image and its accompanying details.
- **NeoWs (Near-Earth Object Web Service):** A summary of Near-Earth Objects (NEOs) is displayed, including detailed data for each object, such as size, velocity, miss distance and closest approach. This makes it easy to view key characteristics of each asteroid directly on the dashboard, without the need for additional interactions.
- **Mars Weather:** Mars weather data is shown in a user-friendly widget, displaying the temperature, atmospheric pressure and wind speed. It also includes the season, min and max temperature. 

### How to Create a Custom Dashboard

1. **Access Home Assistant Settings**
   - Open Home Assistant.
   - In the left sidebar, click on the **Settings** icon (gear symbol).

2. **Navigate to Dashboards**
   - Under **Settings**, click on **Dashboards**. This will show you a list of all existing dashboards in your Home Assistant setup.

3. **Create a New Dashboard**
   - In the **Dashboards** section, click the **+ ADD DASHBOARD** button at the bottom left.
   - A pop-up will appear. Select **New dashboard from scratch**.

4. **Configure the Dashboard Settings**
   - In the **Dashboard Title** field, enter a name for your dashboard (e.g., "NASA").
   - For the **Icon**, select **mdi:rocket-launch** or any other relevant icon.
   - Click **Create** to create the new dashboard.

5. **Open the New Dashboard**
   - After creating the dashboard, a new tab labeled **NASA** (or the name you chose) will appear in the upper section.
   - Click on this tab to open the newly created dashboard.

6. **Edit the Dashboard Configuration**
   - In the **NASA** dashboard page, click on the three vertical dots (more options) in the top-right corner.
   - Select **Raw configuration editor** from the dropdown menu.

7. **Use the Suggested Configuration Below**
   - Copy the configuration provided below and paste it into the **Raw configuration editor**.

8. **Save the Configuration**
   - After pasting the configuration, click **Save** to apply the changes.

9. **Finish Setup**
   - Once the configuration is saved, click **Done** at the top of the page to complete the dashboard setup.

### Recommended configuration for Custom Dashboard

```YAML
views:
  - title: Home
    sections:
      - type: grid
        cards:
          - type: heading
            heading: Near-Earth Objects
            heading_style: title
            icon: mdi:alert-circle
            badges:
              - type: entity
                entity: sensor.near_earth_objects_total_neo_count
          - type: markdown
            title: List of Near-Earth Objects
            content: >
              {% set asteroids =
              state_attr('sensor.near_earth_objects_total_neo_count',
              'asteroids') %} {% if asteroids %}

              {% for asteroid in asteroids %} ## **{{ asteroid.name }}**

              - **ID:** {{ asteroid.id }}

              - **Potentially Hazardous:** {{ asteroid.hazardous }}

              - **Diameter (min):** {{ asteroid.min_diameter_m }} m

              - **Diameter (max):** {{ asteroid.max_diameter_m }} m

              - **Close Approach Date:** {{ asteroid.close_approach_date }}

              - **Miss Distance:** {{ asteroid.miss_distance_km }} km

              - **Relative Velocity:** {{ asteroid.relative_velocity_kph }} km/h

              [See more]({{ asteroid.url }})

              ___

              {% endfor %} {% else %} No asteroid data available. {% endif %}
          - type: entities
            entities:
              - entity: sensor.near_earth_objects_total_neo_count
                name: Total NEO Count
              - entity: sensor.near_earth_objects_potentially_hazardous_neos
                name: Potentially Hazardous NEOs
              - entity: sensor.near_earth_objects_smallest_neo_diameter
                name: Smallest NEO Diameter
              - entity: sensor.near_earth_objects_average_neo_diameter
                name: Average NEO Diameter
              - entity: sensor.near_earth_objects_largest_neo_diameter
                name: Largest NEO Diameter
              - entity: sensor.near_earth_objects_slowest_velocity
                name: Slowest Velocity
              - entity: sensor.near_earth_objects_fastest_velocity
                name: Fastest Velocity
              - entity: sensor.near_earth_objects_closest_approach_distance
                name: Closest Approach Distance
              - entity: sensor.near_earth_objects_farthest_approach_distance
                name: Farthest Approach Distance
            state_color: false
            title: Summary
        column_span: 2
      - type: grid
        cards:
          - type: heading
            heading: Astronomy Picture of the Day
            heading_style: title
            icon: mdi:image-frame
            badges:
              - type: entity
                entity: image.astronomy_picture_of_the_day
          - type: markdown
            content: |
              ## {{ state_attr('image.astronomy_picture_of_the_day', 'title') }}
          - type: picture-entity
            entity: image.astronomy_picture_of_the_day
            show_name: false
            show_state: false
            name: |
              {{ state_attr('image.astronomy_picture_of_the_day', 'title') }}
            caption: >
              {{ state_attr('image.astronomy_picture_of_the_day', 'description')
              }}
          - type: markdown
            content: >
              {{ state_attr('image.astronomy_picture_of_the_day', 'description')
              }}
          - type: markdown
            content: >
              ## 🌌 Mars Weather Overview

              **Sol**: {{ state_attr('weather.mars_weather', 'sol') }}  

              **Season**: {{ state_attr('weather.mars_weather', 'season') }}  


              **🌡️ Temperature**:  

              - Current: {{ state_attr('weather.mars_weather', 'temperature') }}
              {{ state_attr('weather.mars_weather', 'temperature_unit') }}  

              - Min: {{ state_attr('weather.mars_weather', 'temperature_min') }}
              {{ state_attr('weather.mars_weather', 'temperature_unit') }}  

              - Max: {{ state_attr('weather.mars_weather', 'temperature_max') }}
              {{ state_attr('weather.mars_weather', 'temperature_unit') }}  


              **🌀 Atmospheric Pressure**:  

              {{ state_attr('weather.mars_weather', 'pressure') }} {{
              state_attr('weather.mars_weather', 'pressure_unit') }}  


              **💨 Wind**:  

              - Speed: {{ state_attr('weather.mars_weather', 'wind_speed') }} {{
              state_attr('weather.mars_weather', 'wind_speed_unit') }}  

              - Bearing: {{ state_attr('weather.mars_weather', 'wind_bearing')
              }}° 
```

---

## Troubleshooting

- Cannot connect to NASA API:
- Ensure your API key is correct and active.
- Verify your network connection.
- No data displayed:
  - Check if entities are properly created and visible in Home Assistant.
  - Ensure you’ve enabled the APIs in the configuration.
  - Rate limiting:
  - NASA’s free API key has limits. If you hit them, request an upgraded API key from NASA.

---

## Additional Notes

- API data availability is subject to NASA’s uptime and performance.
- Update intervals for sensors can be modified in coordinator.py.
