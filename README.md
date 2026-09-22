# Using Analytics Driven OpenWeather-Rainwater-Harvesting-Integration in the process of gathering crops, other produce from a field, garden and farming.


[![Repository Achievement](https://img.shields.io/badge/Repository-Achievement%20%7C%20Accessible%20%7C%20Findable%20%7C%20Reproducible%20%7C%20Interoperable-4B7BE5?logo=github)](https://github.com/Nkdarmel/OpenWeather-Rainwater-Harvesting-Integration/#repository-achievement)
<p align="center">
  <img alt="Repository Achievement" src="https://img.shields.io/badge/Repository%20Achievement-Research%20Simulation%20Ready-0A7EA4?style=for-the-badge&logo=github" />
</p>

The project is inspired by FAIR research practices and focuses on **feasibility, accessibility, interoperability, and reproducibility** rather than claiming a platform-issued GitHub achievement.

<p align="center">
  <a href="#feasible"><img alt="Feasible" src="https://img.shields.io/badge/Feasible-research%20prototype-2E7D32?style=flat-square" /></a>
  <a href="#accessible"><img alt="Accessible" src="https://img.shields.io/badge/Accessible-documented-1565C0?style=flat-square" /></a>
  <a href="#interoperable"><img alt="Interoperable" src="https://img.shields.io/badge/Interoperable-Python%20workflow-6A1B9A?style=flat-square" /></a>
  <a href="#reproducible"><img alt="Reproducible" src="https://img.shields.io/badge/Reproducible-versioned%20workflow-E65100?style=flat-square" /></a>
</p


[![GitHub](https://img.shields.io/badge/GitHub-%23181717.svg?&style=flat-square)](https://github.com/yourusername/OpenWeather-Rainwater-Harvesting-Integration)

 
 Integrate OpenWeather API with Rainwater Harvesting Storage System (RWHSS) using Python. Fetch weather data, predict precipitation and optimize storage levels based on morning/afternoon hours. Simulate RWHSS operations for Ouagadougou city.

## Foreword

As the world grapples with the challenges of climate change, water scarcity, and urbanization, innovative solutions are needed to ensure sustainable management of our most precious resource: water. Rainwater harvesting (RWH) has emerged as a promising approach to mitigate these issues by collecting and utilizing rainwater for various purposes such as irrigation, toilet flushing, and even drinking [1].

However, RWH systems often rely on manual monitoring and control, which can be time-consuming, labor-intensive, and prone to errors. The integration of weather forecasting data with RWH systems has the potential to revolutionize this process by providing real-time insights into precipitation patterns, allowing for more effective collection and utilization of rainwater. The OpenWeather-Rainwater-Harvesting-Integration project aims to develop a cutting-edge system that combines open-source weather forecasting tools (OpenWeather) with advanced Rainwater-Harvesting (RWH) technologies. This innovative approach will enable users to optimize their Rainwater-Harvesting (RWH) systems in real-time, ensuring maximum water yield while minimizing waste and environmental impact [2].

In this foreword, we highlight the significance of integrating OpenWeather data with RWH systems, drawing from existing research and literature on both topics. We also provide an overview of the project's objectives, methodology, and expected outcomes [1].


## Prerequisites

-Install `requests` and `json` libraries: `pip install requests json`

-Sign up for an OpenWeather API account and obtain your API key. Set the API key as an environment variable or hardcode it in the code [2].

-Familiarize yourself with the OpenWeather API documentation, particularly the "Current Weather" endpoint [3].

### Code

weather_api.py

```python
import requests
import json

def get_weather_data(api_key, city):
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}"
    response = requests.get(url)
    data = json.loads(response.text)
    return data

if __name__ == "__main__":
    api_key = "YOUR_ API_KEY_HERE"
    city = "Ouagadougou"
    weather_data = get_weather_data(api_key, city)
    print(json.dumps(weather_data, indent=4))
```

rwhss.py

```python
import datetime
from weather_api import get_weather_data

class RWHSS:
    def __init__(self):
        self.storage_capacity = 1000   # liters
        self.current_storage_level = 0
        self.weather_data = None

    def update_weather_data(self, city):
        api_key = "YOUR_API_KEY_HERE"
        weather_data = get_weather_data(api_key, city)
        self.weather_data = weather_data

    def predict_precipitation(self):
        if self.weather_data and self.weather_data["main"]["precipitation"]:
            precipitation_amount = float(self.weather_data["main"]["precipitation"])
            return precipitation_amount
        else:
            return 0.0

    def optimize_storage_level(self, precipitation_amount):
        current_time = datetime.datetime.now()
        if precipitation_amount > 0 and current_time.hour < 12:   # morning hours
            self.current_storage_level += int(precipitation_amount * 0.5)
        elif precipitation_amount > 0:
            self.current_storage_level += int(precipitation_amount)

    def get_current_storage_level(self):
        return self.current_storage_level

if __name__ == "__main__":
    rwhss = RWHSS()
    city = "Ouagadougou"
    rwhss.update_weather_data(city)
    precipitation_amount = rwhss.predict_precipitation()
    print(f"Predicted precipitation: {precipitation_amount} mm")
    rwhss.optimize_storage_level(precipitation_amount)
    print(f"Current storage level: {rwhss.get_current_storage_level()} liters")
```

main.py

```python
import datetime

def main():
    city = "Ouagadougou"
    weather_data = get_weather_data(api_key, city)

    rwhss = RWHSS()
    rwhss.update_weather_data(city)
    precipitation_amount = rwhss.predict_precipitation()

    if precipitation_amount > 0:
        print(f"Predicted precipitation: {precipitation_amount} mm")
        rwhss.optimize_storage_level(precipitation_amount)

    current_time = datetime.datetime.now()
    if current_time.hour < 12:   # morning hours
        print("Morning! Adjusting storage level...")
    else:
        print("Afternoon! No adjustments needed.")

if __name__ == "__main__":
    main()
```

### Running the Code

1. Install dependencies using `pip install requests json`
2. Set your OpenWeather API key as an environment variable or hardcode it in the code.
3. Run the script: `python main.py` [4].

### References

[1] GitHub repository for this project.

[2] OpenWeather API documentation, "Getting Started" section.

[3] OpenWeather API documentation, "Current Weather" endpoint.

Brisson, J., & Tuller, S. (2017). Rainwater harvesting: A review of the current state-of-the-art. Journal of Cleaner Production, 164, 1075-1086.

Goyal, M. K., Kumar, P., & Singh, R. (2020). Integration of weather forecasting and rainwater harvesting for sustainable water management. Water Resources Management, 34(10), 3471-3484.

Kumar, S., & Mishra, V. K. (2019). Rainfall-based real-time control strategy for optimal operation of a decentralized rainwater harvesting system. Journal of Hydroinformatics, 21(2), 241-253.

OpenWeather API documentation link:https://openweathermap.org/api

Python requests library documentation link:https://docs.python-requests.org/en/master/

Python json library documentation link:https://docs.python.org/3/library/json.html


Bibliography

"OpenWeather API" by OpenWeather, Inc.

"Requests: HTTP for Humans" by Kenneth Reitz.

"JSON in Python" by Guido van Rossum.

American Meteorological Society. (2020). Glossary of meteorology.

Australian Government Department of the Environment and Energy. (2019). Rainwater harvesting: A guide to designing and installing a rainwater tank system.

International Association for Urban Drainage Research. (2018). Rainwater harvesting systems: Design, installation, and maintenance guidelines.
