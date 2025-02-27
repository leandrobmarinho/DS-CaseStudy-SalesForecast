# 📊 **Demand Forecasting for Walmart Sales**

---

## 📝 **Introduction**

Demand forecasting plays a pivotal role in supply chain management and inventory optimization. Accurate forecasts enable businesses to maintain optimal stock levels, reduce operational costs, and improve customer satisfaction. This project focuses on predicting Walmart sales across different stores and product categories for a 28-day horizon using machine learning, specifically the **XGBoost** algorithm, compared with baseline models based on daily and weekly sales averages.

The project is structured into two main notebooks:
- `pipeline_forecasting.ipynb`: Implements the complete forecasting pipeline using **XGBoost**.
- `reference_model.ipynb`: Provides baseline forecasts based on historical averages (daily and weekly).

---

## 🚀 **Proposed Approach**

### 📦 **Data Exploration and Preprocessing**

The dataset includes:
- **Sales data:** Historical daily sales per item and store.
- **Calendar events:** Dates of special events and holidays.
- **Price information:** Historical selling prices per item.

Key preprocessing steps:
✅ Inspected and cleaned data to identify missing values and anomalies.  
✅ Merged datasets into a single DataFrame containing sales, prices, and event indicators.  
✅ Transformed categorical variables using **XGBoost**'s built-in handling of categorical features.  

#### 📂 **Test Data Processing**

> 📝 **New Script Created:**  
> A separate script, `process_test.py`, was developed to handle test data processing independently from the training pipeline.  
> 🔑 **Key Difference:**  
> - The **training data** underwent filtering of recent dates to improve performance.  
> - The **test data** retained the **full historical record** to ensure forecasts incorporate all available information, improving prediction accuracy in real-world scenarios.  

---

## 🧠 **Feature Engineering**

To capture complex sales patterns, we created several features:

### 📅 **Calendar-Based Features**  
- **`wday`**: Day of the week  
- **`month`**: Month of the year  
- **`day_of_month`**: Detects end-of-month trends  
- **`event_indicator`**: Marks special events (holidays, sports events, etc.)  

### 🔄 **Temporal Features**  
- **Lag Features:**  
  - `delta_1`: Difference in sales compared to the previous day  
- **Rolling Statistics:**  
  - 7-day and 15-day rolling means and standard deviations to capture recent trends.  
- **Cumulative Sums:**  
  - `sum_7`, `sum_15`: Aggregate sales over 7 and 15-day windows.  

### 💵 **Other Features:**  
- **`sell_price`**: Latest item price  
- **`snap`**: Consolidated SNAP benefit indicator  

---

## 🧪 **Model Selection and Training**

### 🤖 **Why XGBoost?**  
✅ High efficiency on large datasets  
✅ Built-in categorical feature support (with `enable_categorical=True`)  
✅ Quick hyperparameter tuning with `early_stopping_rounds`  
✅ Robust performance on tabular data  

### ⚙️ **Model Configuration:**  
- **`enable_categorical=True`**: Direct handling of categorical variables  
- **`early_stopping_rounds=10`**: Prevents overfitting  
- **`min_child_weight=5`**: Controls overfitting by requiring a minimum number of samples per split  

### 🗓️ **Training and Validation Setup**

| Dataset                | Date Range          | Purpose           |
|------------------------|---------------------|-------------------|
| `sales_train_validation` | 2011-01-29 to 2016-04-24 | Model training    |
| `sales_test_validation`  | 2016-04-25 to 2016-05-22 | Model validation  |
| `sales_test_evaluation`  | 2016-05-23 to 2016-06-19 | Final predictions |

---

## 📈 **Forecast Evaluation**

### 📊 **Why Mean Absolute Error (MAE)?**

We chose **MAE** because:  
✅ **Simple and Intuitive:** Reflects the average absolute forecast error.  
✅ **Easy Interpretation:** Expressed in the same units as sales, making it business-friendly.  
✅ **Robust to Outliers:** Less sensitive to large errors compared to RMSE.  

---

## 🏆 **Results**

### 📅 **Reference Model: Baseline Forecasts**

We implemented two baseline approaches:

#### 1️⃣ **Daily Average Forecast**  
- Computes the average sales per `item_id`, `store_id`, and `weekday`.  
- Provides a quick benchmark reflecting weekly sales cycles.  

#### 2️⃣ **Weekly Average Forecast**  
- Computes the average sales per `item_id`, `store_id`, and `week`.  
- Captures broader sales trends but is less granular than daily averages.  

#### 📉 **Baseline Model Performance:**

| Model                  | MAE    |
|------------------------|--------|
| **Daily Average**      | 1.1315 |
| **Weekly Average**     | 1.2791 |

> ⚠️ **Observation:**  
> The daily forecast outperformed the weekly approach.  
> **Reason:** Weekly averages lacked the granularity to capture short-term variations.  

---

### 🚀 **Pipeline Forecasting (XGBoost Model)**

After training the **XGBoost** model with engineered features:

| Dataset     | MAE    |
|-------------|--------|
| **Validation** | 0.52   |
| **Test**       | 0.54   |

#### 📊 **Comparison with Baselines:**

| Model                  | MAE    | Improvement over Baseline |
|------------------------|--------|---------------------------|
| **Pipeline Forecast (XGBoost)** | 0.54   | **52.3%** vs Weekly, **52.3%** vs Daily |
| **Daily Average**      | 1.1315 | Baseline                   |
| **Weekly Average**     | 1.2791 | Baseline                   |

✅ **XGBoost reduced forecast errors by over 50%** compared to both baselines.  

---

## 📊 **Visual Comparisons**

### 🛍️ **Example: Product & Store Comparison**

#### 📅 Actual vs Forecasted Sales (Daily Average Baseline)
![Daily Forecast](https://via.placeholder.com/600x300?text=Daily+Forecast+Plot)  

#### 🚀 Actual vs Forecasted Sales (XGBoost Model)
![Pipeline Forecast](https://via.placeholder.com/600x300?text=Pipeline+Forecast+Plot)  

> 🎯 **Takeaway:** The pipeline model consistently tracks actual sales better, especially during spikes.

---

## 📅 **Daily MAE Trends**

### 📊 **Day-by-Day Forecast Error (Test Period)**

| Day      | Daily Forecast MAE | Weekly Forecast MAE | XGBoost MAE |
|----------|--------------------|---------------------|-------------|
| d_1942   | 1.3245              | 19.7710             | 0.5327      |
| d_1947   | 4.3766              | 19.6352             | 0.5361      |
| d_1954   | 4.3482              | 20.5216             | 0.6492      |
| d_1969   | 5.3264              | 22.6757             | 0.5671      |

> 🔍 **Insight:**  
> - **Baseline models** suffer from high forecast errors, particularly on later days.  
> - **XGBoost forecasts** remain stable, even as the prediction horizon extends.  

---

## 🏗️ **Project Structure**

```plaintext
project_root/
├── notebooks/
│   ├── pipeline_forecasting.ipynb  ← Full pipeline with XGBoost
│   └── reference_model.ipynb       ← Baseline models (daily & weekly averages)
├── scripts/
│   └── process_test.py             ← Handles test data preprocessing
├── data/
│   ├── train.snap.parquet          ← Processed training data
│   └── test.snap.parquet           ← Processed test data
├── models/
│   └── xgb_model.pkl               ← Trained XGBoost model
├── results/
│   └── final_predictions.csv       ← 28-day forecast outputs
```

---

## 🧩 **Next Steps**

✅ **Feature Enrichment:** Add promotional data, competitor pricing, and weather info.  
✅ **Hyperparameter Optimization:** Further tune XGBoost for marginal gains.  
✅ **Production Deployment:** Automate forecasts with real-time data feeds.  
✅ **Retraining Strategy:** Implement periodic model updates to address data drift.  
e retailers like Walmart, ensuring optimal inventory management and enhanced customer satisfaction.
