# Air Quality Pipeline (Shanghai Jiading–Nanxiang)

Our Lab1 using **Shanghai Jiading Nanxiang** as the primary monitoring station and adding new **3-day rolling mean weather features**.  
By performing additional data cleaning, handling missing values, and aligning dates across datasets, the overall data quality improves, leading to better model performance.

---



### **E Part – Use Shanghai Jiading–Nanxiang AQ Data**

We use the **Shanghai Jiading–Nanxiang** station as the data source because its AQ readings are stable and complete.

**Data flow:**
1. Download Jiading–Nanxiang AQ data.Use PM2.5 from Jiading–Nanxiang as **y_label**.  
2. Download weather data,use weather features as **X_train**.  
4. Register the cleaned features in **Hopsworks Feature Store**.  
5. Train **XGBoost** models using these registered features.  
6. Register the trained models in **Hopsworks Model Registry**.  
7. Configure a github **Action** to trigger inference when new weather data arrives.  
8. Display predictions and trends on a **github Page dashboard**.


---

### **C Part – Add Weather Feature and Incremental Inference Engineering**
Weather feature processing is extended with additional derived features, and the inference workflow has fundamentally shifted from a batch processing mode to an **incremental** approach. 

#### 1. **Historical Weather Feature Augmentation**
Added in `weather-features` derived from historical air quality data:

- `pm25_3day_mean`: 3-day mean pm25   

Implementation details:

- Forward fill (FFill) for cases where consecutive values are missing   
- Use `rolling(window=3, min_periods=1).mean().shift(1)` to compute the 3-day moving averages  
- Merge with original weather feature based on the date column  

#### 2. **Daily Updated New Features**
Since the original feature(weather) and air quality(label) will update daily, the new added feature will also update daily by:
- Fetch the historical air quality data covering the **seven days preceding today** (T-7 to T-1).
- Same implementation as in 1

#### 3. **Incremental Inference**
To predict the air quality for the next seven days, a chained inference is employed:
- the new predctions are used to calculate the new `pm25_3day_mean` feature for the next day.

---

## Training – Improved Accuracy

After adding the new features Accuracy improved

