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
- Backward fill (BFill) for cases where consecutive values are missing  

#### 3. **Date alignment across datasets**
Because AQ and weather datasets may have different date coverage:

- A unified daily date index is created  
- A full outer join aligns AQ and weather data  
- Missing values are filled afterward  


---

## Training – Improved Accuracy

After adding the new features Accuracy improved

