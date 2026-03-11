# **Olist-E-commerce-Data-Analysis**
--

## **INTRODUCTION**
This project was developed as the final project for the Ironhack Data Analytics Bootcamp. The objective of the project is to apply the knowledge and technical skills acquired during the program to perform an end-to-end data analysis workflow using a real-world dataset.

The dataset used in this project comes from the Brazilian E-Commerce Public Dataset by Olist, which contains information about orders, customers, payments, reviews, products, and sellers from a Brazilian marketplace. The dataset provides an opportunity to explore customer behavior, order patterns, and business insights within an e-commerce environment.

Through this project, I demonstrate my ability to collect, clean, analyze, and visualize data, as well as apply statistical methods and machine learning techniques to extract actionable insights.


## **THE SCOPE OF THIS TASK**
### This project follows the IronHack bootcamp final project guideliens:
1. Dataset Selection: Choosing a real-world dataset relevant to business or analytics problems.
2. Data Cleaning and Preparation: Preparing the dataset for analysis by handling missing values, duplicates, and inconsistencies.
3. Exploratory Data Analysis (EDA): Investigating the data to understand distributions, relationships, and trends.
4. Statistical Analysis: Applying statistical techniques to test hypotheses and validate observations.
5. Machine Learning Application: Implementing a supervised learning model to generate predictions or classifications.
6. Data Visualization: Creating clear and informative visualizations to communicate insights.
7. Presentation of Insights: Delivering conclusions and recommendations based on the analysis.




## **DATASET AND DATA INFORMAITON**
The Olist [Source kaggle dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/) comprises nine separate datasets, all of which are stored in CSV format:
1. olist_order_items_dataset
- This dataset includes data about the items purchased within each order.
2. olist_orders_dataset
- This is the core dataset. From each order you find all order information.
3. olist_order_payments_dataset
- This dataset includes data about the orders payment options.
4. olist_order_reviews_dataset
- This dataset includes data about the reviews made by the customers. Built off satisfaction survey on purcahse experence and other comments
5. olist_products_dataset
- This dataset includes data about the products sold by Olist.
6. olist_customers_dataset
- This dataset identifies unique customers in the orders dataset and to find the orders delivery location.
7. olist_sellers_dataset
- This dataset includes data about the sellers that fulfilled orders made at Olist.
8. olist_geolocation_dataset
- This dataset has information Brazilian zip codes and its lat/lng coordinates.
9. product_category_name_translation
- Translates the product_category_name to english.


## **Data Cleaning & Preparation**

Before performing RFM (Recency, Frequency, Monetary) analysis and calculating Customer Lifetime Value (CLV), the dataset required several preprocessing and cleaning steps to ensure data quality and analytical accuracy.

**Initial Data Cleaning**

- Removed duplicate records across key transactional tables to avoid double-counting orders or payments.
- Checked and handled missing values in critical fields such as:
    - customer_id
    - order_id
    - order_purchase_timestamp
    - payment_value
- Converted date fields (e.g., order_purchase_timestamp, order_delivered_customer_date) to datetime format for time-based analysis.
- Standardized column names and formats to ensure consistency across merged tables.
- Filtered out canceled or unavailable orders where necessary to focus on completed transactions.


## **Dataset Integration**
Since the dataset consists of multiple relational tables, the following merges were performed:

- Joined orders and customers tables to associate purchases with individual customers.
- Merged order_items to calculate product-level transaction values.
- Integrated order_payments to obtain accurate payment amounts per order.
- Aggregated order-level data to construct a customer-level transaction dataset.



## **Data Preparation for RFM Analysis**

To build the Recency, Frequency, and Monetary (RFM) model, the dataset was transformed into a customer-centric structure.

Cleaning and preparation steps included:

- Aggregated transaction data at the customer level.
- Calculated Recency as the number of days since the customer’s most recent purchase relative to the dataset’s latest transaction date.
- Calculated Frequency as the total number of completed orders per customer.
- Calculated Monetary Value as the total revenue generated per customer based on payment values.
- Removed customers with incomplete purchase records or invalid transactions.
- Verified that monetary values were positive and consistent across payment records.


## **Data Preparation for Customer Lifetime Value (CLV)**

Additional preprocessing was required to estimate both historical and predictive CLV.

Steps included:

- Created customer transaction timelines by sorting orders chronologically.
- Calculated customer tenure based on the time between the first and last recorded purchase.
- Computed average order value (AOV) per customer.
- Calculated purchase frequency metrics to support CLV estimation models.
- Removed customers with insufficient transaction history where predictive modeling would not be reliable.
- Structured the dataset to support probabilistic models used in predictive CLV analysis.


## **Machine Learning & Feature Engineering**

To complement the exploratory analysis and customer segmentation work (RFM and CLV), a machine learning workflow was developed to model customer churn behavior within the e-commerce dataset.

**Churn Definition**

A key challenge in this dataset is that most customers make only a single purchase, which can distort churn modeling. To create a meaningful prediction task, churn was defined using the following framework:

- Customers with only one purchase were excluded from the modeling dataset.
- Only customers with two or more historical purchases were considered, ensuring observable purchasing behavior.
- A 90-day inactivity window was used to define churn:
    - Churn = 1 → No purchase within 90 days after the last observed order.
    - Retained = 0 → Customer placed another order within that window.

This approach ensures that churn represents true disengagement rather than normal single-purchase behavior typical in marketplaces.


**Feature Engineering**

Several behavioral and transactional features were engineered to better capture customer engagement dynamics. These features extend beyond traditional RFM metrics and focus on purchase timing and activity patterns.

Key engineered features included:

- Customer Lifetime
    - Time between the first and last purchase.
- Purchase Frequency
    - Total number of completed orders per customer.
- Average Order Value (AOV)
    - Average revenue generated per order.
- Interpurchase Time
    - Average time between consecutive purchases.
- Purchase Intensity
    - Orders per unit of customer lifetime.
- Recency Metrics
    - Time since the last purchase relative to the dataset cutoff.
- Engagement Variability
    - Variation in time between purchases to capture irregular purchasing patterns.

These engineered variables were designed to capture behavioral signals associated with customer retention and disengagement.




**Machine Learning Models**

Multiple supervised learning models were implemented to evaluate churn prediction performance.

**Logistic Regression**
Logistic Regression was used as a baseline classification model due to its interpretability and efficiency. It provides a reference point for understanding the predictive value of engineered features.

**Random Forest**
A Random Forest classifier was implemented to capture nonlinear relationships and interactions between behavioral variables. Tree-based models are well-suited for structured customer transaction data and can automatically detect complex patterns.



**Handling Class Imbalance**

Because churned customers represented the majority class in the dataset, techniques were implemented to address class imbalance:

- Class-weighted learning was applied during model training.
- SMOTE (Synthetic Minority Over-sampling Technique) was tested to synthetically generate additional samples of the minority class.

Model performance was compared to determine whether synthetic balancing improved predictive performance.


**Model Evaluation**

Models were evaluated using metrics appropriate for imbalanced classification problems, including:

- ROC-AUC – to measure overall discrimination ability
- Precision–Recall AUC – to assess model performance on the minority class

This evaluation framework ensures that models are assessed not only on accuracy but also on their ability to correctly identify customers at risk of churn.



## **Full Project Presentation**

For a complete walkthrough of the analysis, methodology, insights, and strategic recommendations, please refer to the full project presentation:

📊 Project Presentation:
(((((Insert presentation link here )))))

The presentation provides detailed explanations of the business problem, analytical workflow, modeling approach, and final recommendations.



## **Key Business Insights**

The analysis of customer transaction behavior revealed several important patterns that influence retention and long-term customer value:

- Customer engagement timing is a key driver of retention. Customers with consistent purchasing rhythms are significantly more likely to remain active on the platform.
- Increasing gaps between purchases are strong early indicators of churn risk, allowing businesses to detect disengagement before customers are permanently lost.
- High spending alone does not guarantee loyalty. Behavioral engagement metrics provide stronger signals of long-term customer value.
- Customer lifetime value varies significantly across segments, highlighting the importance of prioritizing high-potential customers for retention efforts.



##**Strategic Recommendations**

Based on the findings, several data-driven strategies can help improve customer retention and marketplace performance:

- Implement early churn detection systems based on behavioral engagement indicators such as inactivity periods and declining purchase frequency.
- Prioritize retention efforts on high predicted CLV segments to maximize long-term revenue impact.
- Develop targeted re-engagement campaigns for customers showing early signs of disengagement.
- Monitor customer behavioral metrics continuously to support proactive customer relationship management.















## **RECOMMENDATIONS**
**To improve customer retention and address low ratings on certain products, here are some recommendations:**
1. Olist should improve product quality and consistency to meet customer expectations. (Note: Consistently delivering high-quality products builds trust and loyalty among customers.)
2. Olist should enhance customer service by training representatives and implementing efficient support channels. (Note: Providing excellent customer service ensures prompt and effective resolution of concerns, fostering positive customer experiences.)
3. Olist should personalize the customer experience using data and segmentation strategies. (Note: Tailoring recommendations, offers, and communication based on customer insights creates a personalized and engaging experience.)
4. Olist should implement loyalty programs to reward repeat purchases. (Note: Loyalty programs with exclusive discounts and incentives encourage customers to stay engaged and loyal to the brand.)
5. Olist should gather and act on customer feedback to make improvements and enhance satisfaction. (Note: Actively listening to customer feedback allows for identifying areas of improvement and demonstrates a commitment to customer satisfaction.
   
**Regarding the low-rated/high revenue products:**
1. Olist should identify and resolve quality issues through process improvements and rigorous checks. (Note: Analyzing low-rated products helps address recurring quality issues, ensuring better product performance.)
2. Olist should improve customer communication to manage expectations. (Note: Clear and detailed product information helps customers understand the product's features, limitations, and usage, reducing negative experiences.)
3. Olist should proactively address customer concerns by responding to negative reviews. (Note: Promptly and professionally addressing negative reviews shows the company's dedication to resolving issues and improving customer satisfaction.)
4. Olist should continuously improve products based on feedback and market trends. (Note: Regular evaluation and innovation keep products aligned with customer needs and preferences.)
5. Olist should offer exceptional customer service to regain trust and loyalty. (Note: Going above and beyond to resolve issues and provide excellent service can turn negative experiences into positive ones, rebuilding customer trust.)
By implementing these recommendations and considering the accompanying notes, Olist can enhance customer retention, address low ratings, and foster stronger customer relationships.
