# Blood Glucose Forecasting for Type 1 Diabetes


## Competition Details

- **Goal:** Forecast blood glucose levels 1 hour into the future using six hours of preceding data.
- **Dataset Description:**
  - Data was collected from young adults in the UK with type 1 diabetes.
  - **Devices:** Continuous glucose monitors (CGM), insulin pumps, and smartwatches.
  - **Variables include:**
    - Blood glucose readings
    - Insulin dosage
    - Carbohydrate intake
    - Activity data (heart rate, steps, calories)
  - **Structure:**
    - Data is recorded in aggregated five-minute intervals.
    - **Training set:** Samples from the first three months for nine participants (samples are overlapping and in chronological order).
    - **Testing set:** Samples from the remainder of the study for fifteen participants, including unseen ones (non-overlapping, randomly ordered to avoid leakage).
- **Challenges:**
  - Presence of missing values and noise typical of medical data.
  - Heterogeneity due to different device models.
  - Some participants in the test set are not represented in the training data.

## Project Structure

### 1. Data Preprocessing and Feature Engineering
- **Preprocessing Steps:**

- **Time conversion**: Converting time strings to datetime objects.

- **Time feature creation**: Extracting hour and minute, and creating cyclical features (using sine and cosine transformations) to capture time-of-day effects.

- **Handling missing values**: Forward/backward filling for numeric fields.

- **Interpolation for time-series data**: Interpolation for time-series data (especially for blood glucose, insulin, and related measurements).

-**Feature Engineering**:

- Aggregated features are computed over a moving five-hour window. For example, the code calculates both the sum and the mean for blood glucose, insulin dosage, heart rate, steps, and calories.

- Irrelevant features (such as carb and activity data with high noise) are dropped to focus on predictive signals.

### 2. . Modeling Approaches
The pipeline provides multiple modeling options:

- **Gradient Boosting Models**: XGBOOST, CATBOOST, LIGHTGBM

- **Neural Network-Based Approach**: A deep learning model (e.g., using custom DeepTable networks) is implemented to capture non-linear relationships in the data.

- **Custom learning rate scheduler**: Integrated to optimize the training process.

- **Cross-Validation**:

- **GroupKFold**: Ensures that data from the same participant (identified by p_num or similar) stays within a single fold to prevent leakage.

- **Evaluation**:

- **Root Mean Squared Error (RMSE)**: Used to measure prediction performance.

The code reports both fold-level and overall out-of-fold (OOF) RMSE values and includes paired t-tests to compare results across seeds.