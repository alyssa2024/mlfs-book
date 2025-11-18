# Air Quality Pipeline (Shanghai Jiading–Nanxiang)

Our Lab1 using **Shanghai Jiading Nanxiang** as the primary monitoring station and adding new **3-day rolling mean weather features**.  
By performing additional data cleaning, handling missing values, and aligning dates across datasets, the overall data quality improves, leading to better model performance.

---



### **E Part – Use Shanghai Jiading–Nanxiang AQ Data**
In the `aq-backfill` pipeline, we select **Jiading–Nanxiang** (`jiading-nanxiang`) as the default AQ monitoring station.

Key improvements include:

- Fetching only this station’s PM2.5, PM10, AQI, and related indicators  
- Basic cleaning after download (duplicate removal, outlier filtering)  
- Generating a complete continuous date range to ensure daily records  
- Automatically filling missing dates (values will be handled in the features pipeline)

---

### **C Part – Add Weather Feature Engineering**
Weather feature processing is extended with additional derived features.

#### 1. **New: 3-day rolling mean features**
Added in `aq-features`:

- `pm25_3day_mean`: 3-day mean pm25   

Implementation details:

- Sort weather data by date  
- Use `rolling(3).mean()` to compute the 3-day moving averages  
- Merge with AQ data on the date column  

#### 2. **Missing value handling**
To avoid high missing-value ratios during training, we apply:

- Forward fill (FFill) for gaps  
- Backward fill (BFill) for cases where consecutive values are missing  

#### 3. **Date alignment across datasets**
Because AQ and weather datasets may have different date coverage:

- A unified daily date index is created  
- A full outer join aligns AQ and weather data  
- Missing values are filled afterward  
- Ensures the final `features.parquet` is **fully continuous, daily, and complete**

---

## Training – Improved Accuracy

After adding the new features Accuracy improved

