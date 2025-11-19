# Amazon Market Basket Analysis Project README

## Project Overview
This project performs a comprehensive market basket analysis on Amazon transaction data, coupled with customer demographic information. The goal is to identify frequent itemsets, generate association rules, segment customers, and build predictive models for spending behavior. The insights gained can help businesses optimize marketing strategies, personalize recommendations, and improve overall customer engagement.

## Dataset
The analysis utilizes two publicly available datasets from Harvard Dataverse:
*   **`amazon-purchases.csv`**: Contains transaction-level data including product details, purchase prices, quantities, and order dates.
*   **`survey.csv`**: Contains demographic and survey responses linked to `amazon-purchases.csv` via `Survey ResponseID`.

## Project Structure
The project is organized into the following main sections:

### 1. Data Loading
*   Loading `amazon-purchases.csv` and `survey.csv` into pandas DataFrames.
*   Initial inspection of data shapes and types.

### 2. Data Cleaning
*   **`amazon-purchases.csv` (`df`)**: Renaming columns, stripping whitespaces, handling missing values (dropping nulls in 'title', 'code', 'category', imputing 'shipping'), and converting 'date' to datetime format.
*   **`survey.csv` (`sv`)**: Renaming columns for consistency, cleaning and mapping categorical variables (e.g., 'sell_your_data', 'education', 'amazon_use_howmany', 'amazon_use_hh_size'), and ordering 'income' categories.
*   Merging `df` and `sv` into `df_merge` for demographic analysis.

### 3. Exploratory Data Analysis (EDA)
*   **Transactional EDA**: Analyzing top-selling products by quantity and revenue, top-spending users, top categories, most expensive items, and price distributions. Visualizations include bar charts and histograms.
*   **Time-Series Analysis**: Filtering data for 2018-2022 to analyze monthly sales trends and seasonality in product categories like 'BOOT' and 'SANDAL' purchases.
*   **Demographic EDA**: Exploring gender, age, education, household size, income, race, and online purchase frequency distributions. Analyzing substance use and opinions on data selling. Visualizations include bar charts, pie charts, and faceted plots.

### 4. Customer Segmentation
This section focuses on grouping customers based on their purchasing behavior and demographic attributes.

*   **Data Preparation**: Extracting time-based features, calculating total amount spent, and aggregating customer metrics (purchase count, average spending, most frequent category).
*   **RFM Feature Creation**: Calculating Recency, Frequency, and Monetary values for each customer.
*   **Feature Scaling & Transformation**: Handling categorical variables with one-hot encoding, scaling numerical features using `RobustScaler`, and imputing missing values.
*   **Clustering (K-Means)**: Determining the optimal number of clusters using the Elbow Method and Silhouette Scores. Applying K-Means clustering with `optimal_k=2`.
*   **Cluster Analysis**: Characterizing each cluster based on key statistics and visualizing differences in spending, frequency, and other attributes using box plots and scatter plots. T-test confirms significant differences between clusters.
*   **Summary Customer Segmentation**: Defined two segments: **"Power Buyers" (Cluster 0)** (High-income, 35-44 years, high spending/frequency, diverse purchases) and **"Young Frequent Shoppers" (Cluster 1)** (Lower-income, 25-34 years, frequent smaller purchases, lower transaction value).
*   **Customer Spending and Cluster Prediction**: Building a Random Forest Classifier to predict customer segments and a Random Forest Regressor to predict total spending. Evaluating models using `classification_report`, R² score, and RMSE.
*   **Applying Models to New Customers**: Demonstrating how to predict the cluster and spending for new customer profiles.

### 5. Market Basket Analysis (MBA)
This section applies association rule mining to identify product relationships.

*   **Basket Creation**: Filtering for top 1000 popular items and creating a basket matrix (`responseid` vs. `title`).
*   **Frequent Itemset Mining**: Using FP-Growth algorithm to find frequent itemsets.
*   **Rules Filtering**: Generating association rules with high Lift (≥3), Confidence (≥0.7), and Support (≥0.0025).
*   **MBA Visualization**: Visualizing association rules using scatter plots of Support vs. Lift, with Confidence as size/color. Special handling for frequently appearing items like 'gift cards' and 'batteries' to reduce clutter.
*   **Potential Product Concepts**: Developing recommendations for recommendation engines, bundle promotions, and in-store promotion tools based on the generated rules.
*   **MBA with Each Cluster**: Applying MBA separately to "Power Buyers" (Cluster 0) and "Young Frequent Shoppers" (Cluster 1) to derive tailored marketing strategies for each segment.

## Key Findings & Strategic Recommendations

### Customer Segmentation:
*   **Cluster 0: "Power Buyers"**
    *   **Characteristics**: High income ($100K+), 35-44 years old, frequent purchases, high quantity, high transaction value, diverse product interests.
    *   **Strategy**: Target with premium product promotions, exclusive offers, and VIP loyalty programs. Examples: "Buy 3, Get Blackberries Free" bundles, personalized email campaigns, VIP recommendation sections.

*   **Cluster 1: "Young Frequent Shoppers"**
    *   **Characteristics**: Lower income ($25K-$49K), 25-34 years old, frequent but smaller quantity purchases, lower average transaction value, often buy general items.
    *   **Strategy**: Engage with budget-friendly deals, loyalty points, flash sales, and volume discounts. Examples: "Buy 5, Get 1 Free" stamp cards, "Buy X, Get Y% Off" bundles, notifications via LINE, Instagram, or mobile apps.

### Market Basket Analysis:
*   **General Rules**: Identified strong associations, especially among digital gift cards and fresh produce. For instance, customers buying specific Xbox Gift Cards are highly likely to buy other denominations.
*   **Contextual Recommendations**: The MBA provides actionable insights for cross-selling and upselling, enabling businesses to create relevant product bundles and targeted promotions.
*   **Gift Card/Battery Separation**: Separating highly frequent items like gift cards and batteries helps reveal more diverse and interesting associations for other product categories.

## Technologies Used
*   **Python**: Programming Language
*   **Pandas**: Data manipulation and analysis
*   **NumPy**: Numerical operations
*   **Matplotlib, Seaborn, Plotly Express, Plotly Graph Objects**: Data visualization
*   **Scikit-learn**: Machine learning (K-Means, RandomForestClassifier, RandomForestRegressor, preprocessing pipelines)
*   **`mlxtend`**: Market Basket Analysis (Apriori, FP-Growth, association_rules)

## How to Run the Project
1.  **Clone the Repository**: (Assuming this project is in a repository)
    ```bash
    git clone <repository_url>
    cd <repository_name>
    ```
2.  **Install Dependencies**: (If not in a Colab environment)
    ```bash
    pip install pandas numpy matplotlib seaborn plotly scikit-learn mlxtend
    ```
3.  **Data Setup**: Ensure `amazon-purchases.csv` and `survey.csv` are accessible in the specified paths (e.g., `/content/drive/MyDrive/amazon/`). Adjust paths if necessary.
4.  **Execute Notebook**: Run all cells in the Jupyter/Colab notebook sequentially to reproduce the analysis and results.
