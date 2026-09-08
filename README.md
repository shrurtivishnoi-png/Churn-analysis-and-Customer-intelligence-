# 📊 Customer Churn Analysis & Customer Intelligence

## 📌 Project Overview

This project focuses on analyzing customer churn to understand **why customers leave a service, which customer segments have higher churn, and how customer behavior is related to churn**.

The project combines customer, subscription, and support data to create a single analytical dataset and generate business-focused insights.

The analysis was performed using **Python, Pandas, NumPy, SQLite, Matplotlib, Seaborn, and SQL**.

---

## 🎯 Business Problem

Customer churn can directly affect a company's revenue and customer retention.

The main objective of this project is to answer questions such as:

* What is the overall customer churn rate?
* What is the customer retention rate?
* Which subscription plans have higher churn?
* Which states have higher churn rates?
* How much monthly revenue is at risk because of churned customers?
* What is the average customer tenure?
* How frequently do customers raise complaints?
* Is there a relationship between escalations and customer churn?
* Which customers can be categorized as low, medium, or high churn risk?

---

## 🗂️ Dataset

The project uses three related datasets/tables:

### 1. Customer Data

Contains customer-level information such as:

* Customer ID
* Customer Name
* Gender
* Date of Birth
* State
* Country

### 2. Subscription Data

Contains subscription-related information such as:

* Customer ID
* Subscription Start Date
* Renewal Date
* Cancellation Date
* Plan Type
* Contract Type
* Monthly Charges
* Churn Flag

### 3. Support Data

Contains customer support information such as:

* Customer ID
* Complaint Date
* Escalation information
* Complaint details

---

## 🔄 Project Workflow

```text
Raw Excel Data
      ↓
SQLite Database
      ↓
Data Import
      ↓
Data Cleaning
      ↓
Feature Engineering
      ↓
Data Integration / JOIN
      ↓
Exploratory Data Analysis
      ↓
Customer Churn Analysis
      ↓
Visualization
      ↓
Business Insights
```

---

## 🛠️ Tools & Technologies

| Tool / Technology | Purpose                                      |
| ----------------- | -------------------------------------------- |
| Python            | Data analysis and processing                 |
| Pandas            | Data cleaning and manipulation               |
| NumPy             | Numerical operations and feature engineering |
| SQLite            | Database storage and SQL analysis            |
| SQL               | Data querying and aggregation                |
| Matplotlib        | Data visualization                           |
| Seaborn           | Statistical visualization                    |
| Google Colab      | Development environment                      |

---

# 🧹 Data Cleaning

Several data-cleaning operations were performed before analysis.

### Customer Data

* Renamed `name` to `customer_name`
* Removed unnecessary `interests` and `pincode` columns
* Converted `dob` into datetime format
* Standardized gender values such as `Men → Male` and `Women → Female`
* Filled missing country values using the existing state-country relationship

### Subscription Data

Converted the following columns into datetime format:

* `subscription_start_date`
* `renewal_date`
* `cancellation_date`

### Support Data

* Removed unnecessary columns
* Converted `complaint_date` into datetime format
* Created customer-level complaint counts

---

# ⚙️ Feature Engineering

## Churn Flag

A new `churn_flag` feature was created.

```python
df_db_subscription['churn_flag'] = np.where(
    df_db_subscription['cancellation_date'].notna(),
    1,
    0
)
```

Where:

* `1` = Customer churned
* `0` = Customer did not churn

---

## Customer Tenure

Customer tenure was calculated based on the subscription start date.

For churned customers:

```text
Cancellation Date - Subscription Start Date
```

For active customers:

```text
Current Date - Subscription Start Date
```

This allows comparison of customer retention duration.

---

## Churn Risk

Customers were categorized into:

* **Low Risk**
* **Medium Risk**
* **High Risk**

based on their churn score.

---

# 🔗 Data Integration

The three datasets were combined using `customerid`.

The main relationships were:

```text
Customer
   │
   ├── Subscription
   │
   └── Support
```

The final analytical dataset was created by joining:

```python
df_db_subscription
        ↓
df_db_customer
        ↓
df_db_support
```

A complaint count was also calculated at the customer level before merging the support data.

---

# 📈 Key Analysis

The project calculates several important business metrics.

### 1. Churn Rate

Percentage of customers who have churned.

```text
Churn Rate = Churned Customers / Total Customers × 100
```

### 2. Retention Rate

```text
Retention Rate = 100 - Churn Rate
```

### 3. Churn by Plan Type

Churn rate was calculated for each subscription plan.

### 4. Churn by State

Customer churn was analyzed across different states.

### 5. ARPU

Average Revenue Per User was calculated using monthly charges.

### 6. Average Customer Tenure

Average number of days customers remained subscribed.

### 7. Revenue at Risk

Monthly revenue associated with churned customers was calculated.

### 8. Escalation Rate

Percentage of customers associated with escalated cases.

### 9. Average Complaints per User

Average number of complaints per customer.

### 10. Escalation vs Churn

Correlation was calculated between escalation behavior and churn.

---

# 📊 Visualizations

The project includes multiple visualizations to understand customer churn patterns.

### Monthly Churn Trend

Shows the number of churned customers over time.

### Churn Rate by Plan Type

Compares churn rates across different subscription plans.

### Churn Rate by State

Identifies geographic differences in customer churn.

### Correlation Heatmap

Used to explore relationships between selected customer and churn-related variables.

### Pairplot

Used to examine pairwise relationships between selected variables.

### Customer Segmentation

Analyzes monthly charges across plan types, gender, and churn-risk categories.

---

# 📋 Pivot Table Analysis

Pivot tables were also created to compare:

* Plan type
* Monthly charges
* Number of unique customers
* Churn rate

Example:

```python
pd.pivot_table(
    df_visual,
    index='plan_type',
    values=['monthly_charges', 'customerid', 'churn_flag'],
    aggfunc={
        'monthly_charges': 'sum',
        'customerid': 'nunique',
        'churn_flag': 'mean'
    }
)
```

---

# 💡 Business Insights

The analysis is designed to help a business:

* Identify customer segments with higher churn
* Monitor churn trends over time
* Understand differences between subscription plans
* Identify regions with higher churn
* Estimate revenue at risk from customer churn
* Understand customer support behavior
* Identify customers requiring retention attention
* Make data-driven customer retention decisions

---

# 📁 Project Structure

```text
customer-churn-analysis/
│
├── Customer_Churn_Analysis.ipynb
├── customer_churn_data_raw.xlsx
├── customer_churn.db
├── exported_churn_data.csv
└── README.md
```

> **Note:** Large database/raw-data files may be excluded from GitHub if they exceed GitHub's file-size limits or contain sensitive information.

---

# ▶️ How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/customer-churn-analysis.git
```

### 2. Open the notebook

You can run the notebook using:

* Google Colab
* Jupyter Notebook
* JupyterLab

### 3. Install required libraries

```bash
pip install pandas numpy matplotlib seaborn openpyxl
```

SQLite is included with Python, so no separate SQLite installation is required for the notebook workflow.

### 4. Provide the dataset

Place:

```text
customer_churn_data_raw.xlsx
```

in the project directory.

### 5. Run the notebook

Run the notebook cells from the beginning to reproduce the analysis.

---

# 📌 Skills Demonstrated

This project demonstrates practical skills in:

* Data Cleaning
* Data Preprocessing
* Exploratory Data Analysis
* SQL
* SQLite
* Pandas
* NumPy
* Data Visualization
* Feature Engineering
* Data Integration
* JOIN Operations
* Aggregation
* Pivot Tables
* Customer Segmentation
* Business Analysis
* Customer Churn Analysis

---

# 🚀 Future Improvements

Possible improvements for this project include:

* Building an interactive Power BI dashboard
* Adding customer lifetime value analysis
* Creating a churn prediction model using Machine Learning
* Performing deeper cohort analysis
* Identifying the most important churn drivers
* Creating automated monthly churn reports
* Developing a customer retention recommendation system

---

## 👩‍💻 Author

**Shruti Vishnoi**

Aspiring Data Analyst | Python | SQL | Excel | Power BI

---

⭐ If you find this project useful, feel free to explore the repository and connect with me.

