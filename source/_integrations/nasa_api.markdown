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

#### Entities

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

To enhance the user experience and overcome the limitations of the APOD integration, where clicking on the image doesn’t provide additional details, creating a custom dashboard in Home Assistant is **highly recommended**.
A custom dashboard will allow you to display detailed information for all available NASA data sources, offering richer insights and a more interactive experience. As a user, you have the flexibility to customize the dashboard to suit your preferences,
creating an intuitive and personalized interface for monitoring your data.
Home Assistant provides a comprehensive step-by-step guide on how to create a custom dashboard, which can be found here:  [Home Assistant Documentation on Creating Dashboards](https://www.home-assistant.io/lovelace/)

<a href="javascript:void(0);" onclick="document.getElementById('popup').style.display='block'">
  <img src="../../images/integrations/nasa_api/custom_dash.png" alt="Example of a NASA Integration custom dashboard" width="800">
</a>

<div id="popup" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background-color: rgba(0,0,0,0.8); z-index:999;">
  <span onclick="document.getElementById('popup').style.display='none'" style="position:absolute; top:10px; right:25px; color:white; font-size:36px; font-weight:bold;">&times;</span>
  <img src="../../images/integrations/nasa_api/custom_dash.png" style="position:relative; top:50%; left:50%; transform:translate(-50%, -50%); max-width:100%; max-height:100%;">
</div>

*Example of a custom dashboard for the NASA integration in Home Assistant. It displays detailed information about Near-Earth Objects (NEOs), the Astronomy Picture of the Day (APOD), and Mars Weather. The layout is organized and interactive. **Click on the image to view a zoomed-in version.***


---

## Configuring Alerts for the NASA Data Sources in Home Assistant

Initially, the plan was to code notifications  directly into the NASA integration. However, using **Home Assistant’s Automation** feature provided a more flexible solution. Automations allow for customized notifications based on data changes from NEOs or other NASA data sources. When an alert is triggered, an actionable notification will appear in Home Assistant. It will show updated information, such as the new NEO count. It provide quick access to relevant data or more details. For more information on setting up automations, refer to the [Home Assistant Automation Documentation](https://www.home-assistant.io/docs/automation/).

### How to Set Up Notifications in Home Assistant for NEOs integration

Follow these simplified steps to set up notifications for **hazardous NEO** count changes. The same process can be used for other data sources, such as Mars Weather. Just adjust the entity and conditions as needed.

1. Access Automations
   - Open **Home Assistant**.
   - Navigate to **Settings > Automations & Scenes**.
   - Click **+ CREATE AUTOMATION** in the bottom-right corner.
2. Create a New Automation
   - Choose **Start with an empty automation**.
3. Set Up the Trigger
   - Under **Triggers**, click **+ ADD Trigger**.
   - For **Trigger Type**, select **State**.
   - For **Entity**, select `sensor.near_earth_objects_potentially_hazardous_neos`.
   - Set **From** and **To** values for when the count changes (e.g., if it’s greater than zero).
4. Define the Action
   - Under **Actions**, click **+ ADD Action**.
   - Choose **Call Service**.
   - In the **Service** field, select `notify.notify` (or another available notification service).
   - In the **Service Data** field, enter a message like:

        ```yaml
        message: "The count of potentially hazardous NEOs has changed."
        ```

5. Save and Test the Automation
   - Click **Save** to save the automation.
   - Test the automation by triggering it to confirm the notification is sent.

### Example of other Use Cases

- **NEO Close Approach Distance Alert:**
  - **Trigger**: When the **Closest Approach Distance** of any NEO falls below a set threshold (e.g., 1 million km).
  - **Action**: Send a notification with NEO details such as name, distance and close approach date.

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
