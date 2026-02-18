```python
# If these fail, uncomment and run the install lines
# !pip install pooch requests

import requests
import json
import os
import pooch

```


```python
import json

sample_json = '''
{
  "station": "USC00305800",
  "name": "New York Central Park",
  "location": {
    "latitude": 40.7789,
    "longitude": -73.9692
  },
  "observations": [
    {"date": "2023-01-01", "temperature": 32, "precipitation": 0.0},
    {"date": "2023-01-02", "temperature": 28, "precipitation": 0.5},
    {"date": "2023-01-03", "temperature": 35, "precipitation": 0.0},
    {"date": "2023-01-04", "temperature": 38, "precipitation": 0.2},
    {"date": "2023-01-05", "temperature": 41, "precipitation": 0.0}
  ]
}
'''

data = json.loads(sample_json)

print("Station:", data['station'])

```

    Station: USC00305800



```python
# Dates + Temp

print("Date, Temperature")

for obs in data['observations']:
    print(obs['date'], ",", obs['temperature'])

```

    Date, Temperature
    2023-01-01 , 32
    2023-01-02 , 28
    2023-01-03 , 35
    2023-01-04 , 38
    2023-01-05 , 41



```python
total_temp = 0
count = 0

for obs in data['observations']:
    total_temp += obs['temperature']
    count += 1

avg_temp = total_temp / count

print(f"Average temperature: {avg_temp}°F")

```

    Average temperature: 34.8°F



```python
# Real API Example
# Days With Precipitation

import requests

points_url = "https://api.weather.gov/points/40.7128,-74.0060"

response = requests.get(points_url)
points_data = response.json()

forecast_url = points_data['properties']['forecast']

forecast_response = requests.get(forecast_url)
forecast_data = forecast_response.json()

print("\nForecast Periods:\n")

for period in forecast_data['properties']['periods'][:5]:
    print(period['name'])
    print("Temperature:", period['temperature'], period['temperatureUnit'])
    print("Forecast:", period['shortForecast'])
    print("----")
```

    
    Forecast Periods:
    
    This Afternoon
    Temperature: 44 F
    Forecast: Mostly Sunny
    ----
    Tonight
    Temperature: 39 F
    Forecast: Mostly Cloudy
    ----
    Wednesday
    Temperature: 43 F
    Forecast: Light Rain Likely
    ----
    Wednesday Night
    Temperature: 37 F
    Forecast: Chance Light Rain
    ----
    Thursday
    Temperature: 41 F
    Forecast: Cloudy then Slight Chance Light Rain
    ----



```python
# Downloading first file

file_path = pooch.retrieve(
    url="https://github.com/pandas-dev/pandas/raw/main/doc/data/air_quality_no2.csv",
    known_hash=None
)

print("File downloaded to:", file_path)
print("File exists:", os.path.exists(file_path))

```

    Downloading data from 'https://github.com/pandas-dev/pandas/raw/main/doc/data/air_quality_no2.csv' to file '/home/mjs2459/.cache/pooch/458dad453f6a48e510cd544bef1854e3-air_quality_no2.csv'.
    SHA256 hash of downloaded file: 365ca31c9296ac200e73d357e16a3c1340f9ce6746c83bf81403046dcb374361
    Use this value as the 'known_hash' argument of 'pooch.retrieve' to ensure that the file hasn't changed if it is downloaded again in the future.


    File downloaded to: /home/mjs2459/.cache/pooch/458dad453f6a48e510cd544bef1854e3-air_quality_no2.csv
    File exists: True



```python
# Checking file lines
file_size = os.path.getsize(file_path)
print(f"File size: {file_size} bytes")

line_count = 0

with open(file_path, 'r') as f:
    for line in f:
        line_count += 1

print(f"Number of lines: {line_count}")

```

    File size: 31984 bytes
    Number of lines: 1036



```python
# Dowloading second climate data set 

my_url = "https://raw.githubusercontent.com/jbrownlee/Datasets/master/daily-min-temperatures.csv"

my_file = pooch.retrieve(
    url=my_url,
    known_hash=None
)

print("Downloaded file:", my_file)
print("File size:", os.path.getsize(my_file), "bytes")

```

    Downloading data from 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/daily-min-temperatures.csv' to file '/home/mjs2459/.cache/pooch/c419b29307c512529bd0cb058c3661b7-daily-min-temperatures.csv'.
    SHA256 hash of downloaded file: 8b9de63ed6789492bf497625e7f9beb96a63d367b4b0a21754006f749fa5e5da
    Use this value as the 'known_hash' argument of 'pooch.retrieve' to ensure that the file hasn't changed if it is downloaded again in the future.


    Downloaded file: /home/mjs2459/.cache/pooch/c419b29307c512529bd0cb058c3661b7-daily-min-temperatures.csv
    File size: 67921 bytes



```python
print("\nData Inventory:")
print("1. air_quality_no2.csv - Air quality NO2 measurements")
print("2. daily-min-temperatures.csv - Daily minimum temperature dataset")

```

    
    Data Inventory:
    1. air_quality_no2.csv - Air quality NO2 measurements
    2. daily-min-temperatures.csv - Daily minimum temperature dataset



```python
# NetCDF Metadata
## Get DDS

base_url = "http://iridl.ldeo.columbia.edu/expert/SOURCES/.NOAA/.NCEP/.CPC/.UNIFIED_PRCP/.GAUGE_BASED/.GLOBAL/.v1p0/.Monthly/.RETRO/.rain/dods"

dds_url = base_url + ".dds"
response = requests.get(dds_url)

print("Dataset Structure:")
print(response.text[:500])

```

    Dataset Structure:
    Dataset {
        Float32 Y[Y = 360];
        Float32 X[X = 720];
        Float32 T[T = 324];
        Grid {
         ARRAY:
            Float32 rain[T = 324][Y = 360][X = 720];
         MAPS:
            Float32 T[T = 324];
            Float32 Y[Y = 360];
            Float32 X[X = 720];
        } rain;
    } rain;
    



```python
das_url = base_url + ".das"
response = requests.get(das_url)

print("Dataset Attributes:")
print(response.text[:1000])

```

    Dataset Attributes:
    Attributes {
        Y {
            String standard_name "latitude";
            Float32 pointwidth 0.5;
            Int32 gridtype 0;
            String units "degree_north";
        }
        X {
            String standard_name "longitude";
            Float32 pointwidth 0.5;
            Int32 gridtype 1;
            String units "degree_east";
        }
        T {
            Float32 pointwidth 1.0;
            String calendar "360";
            Int32 gridtype 0;
            String units "months since 1960-01-01";
        }
        rain {
            Int32 pointwidth 0;
            String standard_name "lwe_precipitation_rate";
            Float32 file_missing_value -999.0;
            String history "Boxes with less than 0.0% dropped";
            Float32 missing_value NaN;
            String units "mm/day";
            String long_name "Monthly Precipitation";
        }
    NC_GLOBAL {
        String Conventions "IRIDL";
    }
    }
    


## 1. Dimensions and Variables

**Dimension Names:**
- T (Time)
- Y (Latitude)
- X (Longitude)

**Main Variable Name:**- rain




```python
## Code for getting data attributes 

das_url = base_url + ".das"
response = requests.get(das_url)

print("Dataset Attributes:")
print(response.text[:1000])
```

    Dataset Attributes:
    Attributes {
        Y {
            String standard_name "latitude";
            Float32 pointwidth 0.5;
            Int32 gridtype 0;
            String units "degree_north";
        }
        X {
            String standard_name "longitude";
            Float32 pointwidth 0.5;
            Int32 gridtype 1;
            String units "degree_east";
        }
        T {
            Float32 pointwidth 1.0;
            String calendar "360";
            Int32 gridtype 0;
            String units "months since 1960-01-01";
        }
        rain {
            Int32 pointwidth 0;
            String standard_name "lwe_precipitation_rate";
            Float32 file_missing_value -999.0;
            String history "Boxes with less than 0.0% dropped";
            Float32 missing_value NaN;
            String units "mm/day";
            String long_name "Monthly Precipitation";
        }
    NC_GLOBAL {
        String Conventions "IRIDL";
    }
    }
    


## 3. Dataset Documentation

### What does this dataset contain?
This dataset contains monthly global precipitation (rainfall) data.

### What time period does it cover?
The time variable has units "months since 1960-01-01" and contains 324 months.
This corresponds to approximately 1960 to 1986.

### What geographic region does it cover?
The dataset has global coverage. The latitude (Y) and longitude (X) dimensions represent a full global grid.

### What are the units of the main variable?
The units of the main variable "rain" are millimeters per day (mm/day).

