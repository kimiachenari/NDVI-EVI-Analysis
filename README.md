
# 🌱 **Vegetation Analysis using Landsat 8: NDVI & EVI** 🌱

Welcome to an exciting journey into the world of satellite imagery and vegetation monitoring! This project takes you on an exploration of Mashhad's green spaces through the lens of remote sensing. By leveraging **Landsat 8** imagery, we'll calculate **NDVI** (Normalized Difference Vegetation Index) and **EVI** (Enhanced Vegetation Index) to track vegetation health and growth in the city throughout 2021.

With this tool, you'll gain insights into Mashhad's vegetation dynamics, identify areas of environmental change, and create a detailed vegetation profile for the region. From cloud-masked satellite images to time-series visualizations and data exports, this project is all about turning raw data into meaningful insights!

## 🌍 **What Does This Do?**

This script processes **Landsat 8 imagery** to analyze vegetation in Tehran using NDVI and EVI, two powerful vegetation indices. Through **cloud masking**, **spatial clipping**, and **temporal analysis**, we generate time-series charts, export CSV data, and visualize how Tehran’s vegetation evolves during the year 2021.

### Key Highlights:

- **Cloud Masking:** Clean satellite images by removing cloud cover.
- **NDVI & EVI Calculation:** Track vegetation health across the region using two different vegetation indices.
- **Timeseries Visualizations:** Generate visual representations of vegetation change over time.
- **Data Export:** Export aggregated data (mean, max, min values of NDVI/EVI) in CSV format for deeper analysis.

## 🚀 **Getting Started**

### 1. **Sign up for Google Earth Engine (GEE)**

If you haven't already, create a Google Earth Engine account here: [Earth Engine Signup](https://signup.earthengine.google.com/). You'll need this to run the script.

### 2. **Define Your Area of Interest (AOI)**

In this project, we've predefined Tehran as the region of interest (ROI), but feel free to use your own geometry! Upload your polygon or use the built-in Tehran geometry.

```javascript
var geometry = ee.FeatureCollection("projects/ee-/assets/");
```

### 3. **Running the Script**

Once you’ve signed up and prepared your region of interest, head over to the [Google Earth Engine Code Editor](https://code.earthengine.google.com/), paste in the provided script, and hit **Run**! Watch as the script works its magic, processing Landsat imagery and outputting insightful visualizations and data.

---

## 🔍 **How It Works**

### 1. **Cloud Masking**
Clouds are often a major interference in satellite imagery, but we've got you covered! The script automatically masks out the clouds using the **BQA band** from Landsat 8.

```javascript
var maskL8 = function(image) {
  var qa = image.select('BQA');
  var mask = qa.bitwiseAnd(1 << 4).eq(0);
  return image.updateMask(mask);
}
```

### 2. **Filtering and Clipping Landsat Data**
The Landsat 8 data is filtered for the year 2021, focused on Tehran's region, and cloud-masked:

```javascript
var dataset = ee.ImageCollection('LANDSAT/LC08/C01/T1_TOA')
  .filterDate('2021-01-01', '2021-12-30')
  .select('B1','B2','B3','B4','BQA')
  .filterBounds(geometry)
  .map(maskL8);
```

### 3. **Calculate NDVI & EVI**
Vegetation health is captured using two indices: **NDVI** and **EVI**. We calculate them from the red, green, and near-infrared bands:

```javascript
var addNDVI = function(image) {
  var NDVI = image.normalizedDifference(['B4', 'B3'])
    .rename('NDVI')
    .copyProperties(image, ['system:time_start']);
  return image.addBands(NDVI);
};
```

### 4. **Exporting Data**
We've made it easy to take the analysis further. The script exports aggregated data into CSV files:

```javascript
Export.table.toDrive({
  collection: finalEVIi,
  description: 'EVImax_'+startdate+'TO'+enddate,
  fileFormat: 'CSV'
});
```

---

## 📊 **Visualize the Results**

With this project, you get both **visual insights** and **quantitative analysis**. You’ll see **NDVI and EVI maps**, and explore **time-series charts** that show the evolution of vegetation health throughout 2021.

### 1. **Visualization on the Map:**
We generate vibrant visualizations of NDVI using color gradients:

```javascript
var NDVIcolor = {
  min: -1,
  max: 1,
  palette: ['FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718', '74A901', '66A000', '529400', '3E8601', '207401', '056201', '004C00', '023B01', '012E01', '011D01', '011301'],
};
Map.addLayer(image, NDVIcolor);
```
![lst2](https://user-images.githubusercontent.com/104256716/176609541-74284575-5554-4804-bb4b-9fbf8ba76659.png)
![2](https://github.com/user-attachments/assets/3250f089-f757-433c-9115-055e9106f889)
![mean seasonal ndvi](https://github.com/user-attachments/assets/92102645-c46d-484a-8109-33eb5d0d3d49)
### 2. **Timeseries Charts:**
Generate insightful timeseries graphs for NDVI and EVI, showing the maximum, mean, and minimum values over time.

```javascript
print(ui.Chart.image.series(withNDVI, geometry, ee.Reducer.max(), 30));
```

---

## 📈 **What You Get**

- **NDVI & EVI Visualizations:** Colorful maps showing vegetation health over time.
- **CSV Data Exports:** Aggregated statistics for NDVI and EVI (mean, max, min).
  - `EVImean_{startdate}_TO_{enddate}.csv`
  - `EVImax_{startdate}_TO_{enddate}.csv`
  - `EVImin_{startdate}_TO_{enddate}.csv`
- **Timeseries Charts:** Visual representation of the vegetation changes during 2021.

## 📝 **Exported Data**

The data exported includes:
- **NDVI & EVI Mean, Max, Min values** for each date.
- **Time-stamped vegetation data** for detailed analysis.
- **Region-based stats** like mean vegetation health for Tehran.

---

## 🎉 **Future Improvements & Ideas**

- 🌿 **Expand the Analysis:** Add more indices (e.g., SAVI, GNDVI) to get a broader view of vegetation types.
- 💡 **Cloud-Free Compositing:** Implement methods to produce composite images with minimal cloud cover for clearer insights.
- 🤖 **Machine Learning Models:** Apply machine learning to classify vegetation types or detect land use changes over time.

---

## 🚀 **Let’s Get Started!**

Dive into the world of satellite data, explore Tehran’s vegetation dynamics, and transform raw satellite data into valuable insights. If you have any questions or want to contribute, open an issue or submit a pull request!

---

### License

This project is open-source and available under the MIT License.

---

**Happy coding and happy analyzing!** 🌱

