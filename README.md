# Homework Assignment: Data Formats and Access

## Code and work saved in the repo within file hw3.ipynb or hw3.md

## Question 1. Dimensions and Variables
Dimension Names:

T (Time)
Y (Latitude)
X (Longitude)

Main Variable Name:- rain

## Question 2. Code for getting data attributes

```python
das_url = base_url + ".das"
response = requests.get(das_url)

print("Dataset Attributes:")
print(response.text[:1000])

# Output for the code

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

```  
---

## Question 3. Dataset Documentation

### What does this dataset contain?
This dataset contains monthly global precipitation (rainfall) data.

### What time period does it cover?
The time variable has units "months since 1960-01-01" and contains 324 months. This corresponds to approximately 1960 to 1986.

### What geographic region does it cover?
The dataset has global coverage. The latitude (Y) and longitude (X) dimensions represent a full global grid.

### What are the units of the main variable?
The units of the main variable "rain" are millimeters per day (mm/day).
