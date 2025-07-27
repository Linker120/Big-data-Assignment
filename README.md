# 🚖 Uber Fares Dataset Analysis using Power BI

**Student:** Akimana Janviere  
**Student ID:** 26769  
**Course:** Introduction to Big Data Analytics (INSY 8413)  
**Lecturer:** Maniraguha Eric  
**Assignment Date:** July 20, 2025  
**Submission Format:** Comprehensive GitHub Report (Markdown)

---

## 📌 Project Objective

This project aims to analyze the Uber Fares Dataset from Kaggle to gain insights into fare patterns, ride durations, and location-based ride trends. The analysis was done using Python (Pandas) and visualized through an interactive Power BI dashboard.

---

## 🧪 Methodology

The project includes comprehensive visual documentation of the entire development process:

---

### 🔹 **Data Loading Process**

The dataset was loaded and inspected using Pandas:

```python
import pandas as pd

df = pd.read_csv('uber.csv')
df.info()
df.head()
```

**Screenshots:**

![n1](https://github.com/user-attachments/assets/30bd6582-9406-46ff-92b4-28806685a167)  
![n1 output](https://github.com/user-attachments/assets/d8b07088-1164-4893-ad5e-1fc40b6cbb47)

---

### 🔹 **Data Cleaning Steps**

We performed the following cleaning operations:

- Removed null values  
- Filtered negative or zero fares  
- Removed out-of-range coordinates  

```python
df.dropna(inplace=True)
df = df[df['fare_amount'] > 0]
df = df[(df['pickup_longitude'].between(-75, -72)) & (df['pickup_latitude'].between(40, 42))]
df = df[(df['dropoff_longitude'].between(-75, -72)) & (df['dropoff_latitude'].between(40, 42))]
df.reset_index(drop=True, inplace=True)
```

**Screenshots:**

![n2](https://github.com/user-attachments/assets/b4372b4e-421f-44e3-93e9-962a822621c4)  
![n2 output](https://github.com/user-attachments/assets/c3be1eb8-26c8-4fcf-b9f3-3604a27aff3e)

---

### 🔹 **Export Cleaned Dataset**

The cleaned dataset was saved for import into Power BI:

```python
df.to_csv('uber_cleaned.csv', index=False)
```

---

### 🔹 **Feature Engineering**

We extracted new time-based features and calculated distances using the Haversine formula:

```python
df['pickup_datetime'] = pd.to_datetime(df['pickup_datetime'])
df['hour'] = df['pickup_datetime'].dt.hour
df['day'] = df['pickup_datetime'].dt.day
df['month'] = df['pickup_datetime'].dt.month
df['year'] = df['pickup_datetime'].dt.year
df['weekday'] = df['pickup_datetime'].dt.day_name()

# Peak hours (7–9 AM, 5–7 PM)
df['is_peak'] = df['hour'].apply(lambda x: 1 if x in range(7, 10) or x in range(17, 20) else 0)

# Distance calculation
from math import radians, cos, sin, asin, sqrt
def haversine(lon1, lat1, lon2, lat2):
    lon1, lat1, lon2, lat2 = map(radians, [lon1, lat1, lon2, lat2])
    dlon = lon2 - lon1 
    dlat = lat2 - lat1 
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    return 6371 * 2 * asin(sqrt(a))

df['distance_km'] = df.apply(lambda row: haversine(
    row['pickup_longitude'], row['pickup_latitude'],
    row['dropoff_longitude'], row['dropoff_latitude']), axis=1)
```

---

## 📊 Power BI Dashboard

### 🎯 Dashboard Features

- Fare Distribution: Histograms and Box Plots  
- Time Series: Hourly, Daily, Monthly Fare Trends  
- Spatial Analysis: Pickup and Drop-off Map  
- Filters: Hour, Day, Month, Passenger Count  
- Trendlines: Peak vs Off-Peak Fares

---

### 🧱 Dashboard Development Stages

**Screenshots:**

![dashboards](https://github.com/user-attachments/assets/5f56d5fb-3bc2-46f3-b7fe-532d2fb99fee)  
![n3](https://github.com/user-attachments/assets/d5330eab-80db-49dd-a72e-5d7ecc580fef)  
![n3 output](https://github.com/user-attachments/assets/31f8b0fc-c2a6-4a89-9f2d-507a8188bdda)

---

### 🔍 Additional Analysis / Process Steps

This includes refining data filters, adjusting map visuals, and final formatting of the dashboard.

**Screenshot:**

![n4](https://github.com/user-attachments/assets/4e04ba58-ee79-44a6-8779-c2295172d76e)

---

## 📈 Insights & Results

- Peak ride demand occurs between **7–9 AM** and **5–7 PM**  
- Friday has the **highest ride volume**  
- Most fares fall between **$5–$15**  
- **Distance and fare** are strongly correlated  
- Pickup hotspots: **Midtown & Downtown Manhattan**

---

## ✅ Conclusion

Through EDA and visualization, we discovered important patterns in Uber ride behavior. Power BI enabled the creation of an interactive dashboard for business insights and data-driven decisions.

---

## 💡 Recommendations

- Increase driver availability during **peak periods**  
- Implement dynamic pricing for long-distance and late-night trips  
- Use spatial data to optimize **driver allocation**

---

## 📬 Submission Information

- **Submitted by:** Akimana Janviere  
- **Student ID:** 26769  
- **Instructor:** Maniraguha Eric  
- **Submission Date:** July 25, 2025  
- **Contact:** [eric.maniraguha@auca.ac.rw](mailto:eric.maniraguha@auca.ac.rw)

---
