# Bank Customer Churn Analytics Dashboard

An end-to-end analytics project: bank customer data was loaded into **SQL Server**, prepared with **T-SQL** views, and visualised in a multi-page **Power BI** dashboard.

The goal is to answer practical retention questions: how many customers leave, which groups leave the most, which current customers are at risk, and what the bank can do about it.

## Tools

- **SQL Server (SSMS):** table creation and loading, data checks, reporting views, risk score
- **Power BI Desktop:** data model, DAX measures, interactive dashboard (bookmarks, key influencers, decomposition tree, and more)

## Data

The source is the public Kaggle dataset *Bank Customer Churn Prediction*: **10,000 customers** and 12 columns.

| Column | Content |
|---|---|
| customer_id | Unique customer number |
| credit_score | Credit score |
| country | France, Germany or Spain |
| gender | Female or Male |
| age | Age in years |
| tenure | Years as a customer |
| balance | Account balance |
| products_number | Number of bank products (1 to 4) |
| credit_card | Has a credit card (1 / 0) |
| active_member | Active member (1 / 0) |
| estimated_salary | Estimated salary |
| churn | Left the bank (1) or stayed (0) |

**Important:** the dataset has no "reason for leaving" column. The exit reasons shown on the page *Why customers leave* are **simulated for demonstration** (see below). They are not real data.

## Data preparation decisions

- **Quality checks:** no missing values and no duplicate customer IDs (10,000 rows = 10,000 unique customers).
- **Source table is never modified.** The raw data stays in `dbo.Churn_Raw`. All preparation happens in the views `dbo.vw_ChurnReport` and `dbo.vw_ChurnRisk`.
- **Groups created in the view:** age group, balance category, credit score band, tenure group, products group, salary band, member status, churn status.
- **Zero balances are kept:** 3,617 customers (36.2%) have a zero balance. This is normal for this dataset, so they were not removed.
- **Simulated exit reasons:** a separate table `dbo.Churn_Exit_Reasons_Simulated` was generated for the 2,037 churned customers only, with seven reasons grouped into four categories (Price, Service, Product, Customer circumstances). The reasons follow simple rules based on customer characteristics. They are labelled as simulated everywhere they appear.

## Metric definitions

| Metric | Definition |
|---|---|
| Churn rate | Churned customers divided by total customers |
| Balance lost | Total balance of customers who churned |
| % balance lost | Balance lost divided by total balance of all customers |
| Risk score | 1 point for each of: age 41 to 60, inactive member, number of products is not 2, country is Germany, balance above zero (gender is not used) |
| Risk level | Low (0 to 1), Medium (2), High (3), Very high (4 to 5) |
| At-risk customers | Customers who have **not** churned and have a High or Very high risk level |
| Balance at risk | Total balance of the at-risk customers |
| Inactive / active members churn | Churn rate within each member status |

## Dashboard pages

1. **Overview:** KPI cards, churned vs retained, churned customers by country, churn rate by age group. A bookmark arrow switches the first chart between the overall split and the split by country.
2. **Customer segments:** churn rate by country and age group (heat map), by gender, by salary band, and a decomposition tree.
3. **Customer behavior:** inactive vs active churn, age groups by balance and churn, churn by number of products, gauge against the overall rate.
4. **At-risk customers:** customers by risk level, balance at risk by country, and the top 15 at-risk customers to contact.
5. **Why customers leave:** churn by reason category and by reason (**simulated reasons**), and a key influencers visual built on the real columns.
6. **Data quality and notes:** record counts, duplicates, age distribution, balance categories, customers by country.
7. **Recommendations:** six actions linked to the findings.

All pages have slicers (products group, member status, age group, country, gender) and a reset button.

## Key findings

- **2,037 of 10,000 customers (20.37%) churned.** They held **185.59M** of the bank's **764.86M** total balance (**24.26%**).
- **Germany churns the most:** 32.44%, about twice France (16.15%) and Spain (16.67%).
- **Age matters a lot:** churn is 56.21% for ages 51 to 60 and 33.97% for 41 to 50, versus 7.52% for 18 to 30. The highest group is Germany, ages 51 to 60, at 69.55%.
- **Inactive members churn at 26.85%** versus 14.27% for active members.
- **Number of products:** customers with 2 products churn 7.58%, with 1 product 27.71%, and with 3 or more products **85.89%**. Key influencers shows "3 or more products" as the strongest factor (about 4.7 times more likely to churn).
- **Female customers churn more:** 25.07% versus 16.46% for male customers.
- **Salary and credit score have little effect:** churn stays close to 20% across all salary and credit score groups.
- **The risk score separates well:** churn is 4.58% for Low risk and 52.98% for Very high risk.
- **2,799 current customers are at high or very high risk**, holding **327.95M** in balances (35.15% of retained customers).

These are patterns in the data, not proven causes. For example, customers with 3 or more products churn more, but the data cannot show that the products are the reason.

## Recommendations

- Focus retention offers on customers aged 41 to 60 (loyalty benefits, a personal relationship manager).
- Re-activate inactive members with engagement campaigns.
- Investigate Germany: pricing, service quality and competitors.
- Review the product mix: encourage a second product, and review fees and service for customers with 3 or more products.
- Contact the top at-risk customers first, starting with the Top 15 list.
- Review rates, fees and service quality. In the simulated reasons, price (39.62%) and service (28.33%) lead, but this must be confirmed with real exit data (for example, exit surveys).

## Files

- `Churn_Dashboard.pbix`: the Power BI dashboard (open with Power BI Desktop)

## How to open

Download the `.pbix` file and open it with **Power BI Desktop** (free, Windows).
