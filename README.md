# **Winter Rock Operations Optimisation**
# Table of Contents

# Table of Contents

- [Project Background](#1-project-background)
- [Executive Summary](#2-executive-summary)
- [Insights Deep-Dive](#3-insights-deep-dive)
  - [Investigating Sales Trends](#31-investigating-sales-trends)
  - [Forecasting Sales of Year-Round Products](#32-forecasting-sales-of-year-round-products)
  - [Distribution Plan](#33-distribution-plan)
  - [Supplier Choice Based on MAXIMAX, MAXIMIN, and MINIMAX](#34-supplier-choice-based-on-maximax-maximin-and-minimax)
  - [Monte Carlo Simulation to Identify Profit Trends](#35-monte-carlo-simulation-to-identify-profit-trends-under-eu-and-us-supplier)
- [Final Recommendations](#4-final-recommendations)


## **1. Project Background**
Winter Rock is a leading winter sports retailer in the UK, specialising in ski, snow, and climbing equipment. As Winter Rock prepares for the critical six-month period leading to December 2022, it aims to optimise operations and adapt to changing market dynamics.
The company’s key challenges include:
- Understanding historical sales trends, average moving sales for the time-period Jan 2019 to December 2022, and their implications for capacity planning.
- Forecasting demand for year-round products to ensure availability and better capacity planning.
- Minimising distribution costs from newly established distribution hubs in London and Manchester to three regions: Northwest, East Midlands, and West Midlands, to meet growing demand for its products.
- Evaluating supplier options for a new low-cost mountain ski product amidst demand uncertainty.
## **2. Executive Summary**
This report, commissioned by Winter Rock, analyses historical sales data from January 2019 to December 2022 to identify trends, forecast demand, and optimise operations. A 12-month centred moving average indicates year-on-year sales growth, with peaks from October to January, highlighting strong seasonality.

Seasonal Exponential Smoothing (SES) was used for forecasting, achieving a mean percentage error of 0.16%, indicating minimal over-forecasting. The SES output acted as the input for Holt-Winters model. The Holt-Winters model, accounting for trend and seasonality, achieved an accuracy of 0.11%, slightly over-forecasting. These models supported a six-month forecast for July–December 2022.

For expansion into the East Midlands, West Midlands, and Northwest, Winter Rock plans distribution hubs in Manchester and London. Linear programming optimised shipping costs to £99,615 by allocating 2,000 units to the East and 500 to the North from Manchester, and 930 units to the West and 1,700 to the North from London.
Launching a £150 low-cost ski, the company projects sales between 200 and 800 units. Monte Carlo simulations suggest contracting the American supplier, with a 79.1% probability of profits between £22,000 and £10,000. The European supplier, limited to 500 units, poses higher opportunity losses, with a 69.1% profit probability.

Recommendations:
-	Prepare for October–January demand using seasonal insights.
- Use SES for forecasting and monitor fluctuations to improve accuracy.
- Implement optimised shipping allocations to minimise costs.
- Contract the American supplier to maximise profitability.
## **3. Insights Deep-Dive**
###	**3.1 Investigating Sales Trends**
Aggregate sales data was examined using a centered moving average of length 12 (CMA-12) to identify patterns and seasonality. The analysis was followed by calculating 12-month rolling average to compute the de-trended values by dividing the actual sales value by the 12-month rolling average. The 12-month rolling average smooths the data, representing the underlying trend. Dividing the actual sales value by the 12-month rolling average, removes the trend, leaving only the seasonal component. The de-trended value were further arranged in a seasonal matrix to understand the seasonality year-on-year. The following are the key insights of the analysis:

![Original Time Series & CMA](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/f68f4a69fcfd46cefc484fcdaa37f05947dc6b4a/Visualisations/Original%20Time%20Series%20%26%20CMA.png)
- Annual Growth: CMA shows a consistent upward trend, with sales value increasing year on year. Initial CMA value of £788K in early periods grew steadily to £1.09M by December 2022.

![Annual Seasonal Profile](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/2beee3c66148aaa7ba3ec0c3c63ed85b360688f3/Visualisations/Annual%20Seasonal%20Profile.png)

- Seasonality: Median seasonal profile reveals a strong peak in sales value from October to January, with the highest detrended value exceeding 1.28. Off-peak periods exhibit values closer to the average or slightly below, with detrended values dipping as low as 0.78.
The following are the implications of the analysis:
- Inventory control and cost management: Increase inventory during the peak seasons (October – January). Allocate 10% additional stock based on forecasted sales and error measures, as excess inventory increases costs and reduces profit margins.
- Marketing strategies: Implement conservative marketing strategies during off-seasons to control costs, while focusing on high-margin products to maintain profitability. Implement discount strategies on premium products during peak periods of Black Friday Sales and Christmas

### **3.2 Forecasting Sales of Year-Round Products**

![SES Forecast](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/2beee3c66148aaa7ba3ec0c3c63ed85b360688f3/Visualisations/SES.png)

Simple Exponential Smoothing (SES) was applied to forecast sales for year-round products. SES leverages a single smoothing parameter, alpha (α), which ranges between 0 and 1. SES places greater weight on recent data while accounting for all historical data. The optimal alpha value was selected to minimise forecast error measures, including Mean Error (ME), Mean Absolute Error (MAE), and Mean Squared Error (MSE), and strike a balance between responsiveness to recent changes and stability of considering historical trends. The optimal α (smoothing factor) was selected based on minimising in-sample errors. Forecast accuracy was evaluated using various error metrics for both in-sample and out-of-sample periods. The choice of α (alpha) significantly impacts the smoothing process by controlling the weight given to recent versus older data. A value of α = 0.55 was selected as it strikes a balance between responsiveness to recent changes and stability by considering historical trends. This choice was guided by the dataset's characteristics, where sales exhibited consistent seasonality and gradual trends, making it essential to avoid overemphasising short-term fluctuations. The optimal α (smoothing factor) was selected based on minimising in-sample errors. Forecast accuracy was evaluated using various error metrics for both in-sample and out-of-sample periods.

![In-sample Forecast Error](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/8cdcf6fe2e86581fc105089be3498f2b1b409e04/Visualisations/In-sample%20forecast%20error.png)

![Out-of-sample Forecast Error](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/8cdcf6fe2e86581fc105089be3498f2b1b409e04/Visualisations/Out-of%20sample%20error.png)

There are pros and cons of each error measure which are as follows:
- Mean Error (ME): It identifies if the model tends to overestimate or underestimate, but doesn’t capture the magnitude of errors.
- Mean Absolute Error (MAE): It is simple to understand and interpret, but doesn’t penalise large errors.
- Mean Squared Error (MSE): It highlights large errors, useful for critical deviations, but overly sensitive to outliers.
- Mean Percent Error (MPE): Easy to interpret in percentage terms, but can distort results when actual values are close to zero.
- Mean Absolute Percent Error (MAPE): Useful for comparability across datasets with different scales, but skewed by small actual values.
- Root Mean Square Percent Error (RMSPE): Penalises larger deviations efficiently, but can be skewed by extreme small values.

The implications of SES include:
- The low in-sample MAPE (2.20%) highlights percentage-based accuracy, useful for comparability, while RMSPE (2.15%) penalises larger deviations but can be skewed by small values.
- The out-of-sample errors, including a MAPE of 3.27%, show reliability for short-term forecasts. However, SES may struggle with seasonal or complex trends, requiring complementary models for improved accuracy.
- To makeup for seasonal or complex trends, it is advisable to use the Holt-Winters Model that has been developed alongside it.

### **3.3 Distribution Plan**
Winter Rock is well placed to serve the growing interest in their products in the East Midlands, West Midlands, and Northwest regions of the UK. The core issue comes from determining the number of goods to be shipped from the distribution hubs located in Manchester and London. The company’s objective is to minimise shipment costs and ensure the demands are met while efficiently utilising the capacities of its distribution hubs. To address this issue, the data provided by the company was modelled in Excel and subjected to linear programming using Excel Solver, which optimised the total number of goods to be shipped.

**Decision Variables:**
- XME: Number of goods shipped from Manchester to East Midlands
- XMW: Number of goods shipped from Manchester to West Midlands
- XMN: Number of goods shipped from Manchester to Northwest
- XLE: Number of goods shipped from London to East Midlands
- XLW: Number of goods shipped from London to West Midlands
- XLN: Number of goods shipped from London to Northwest

**Objective function**
- Minimise: 15XME + 21XMW + 17XMN + 23.5XLE + 25.5XLW + 22XLBN.
**Constraints**

Capacity Constraints:
- XME + XMW + XMN  <= 2,500
- XLE + XLW + XLN <= 3,000
Demand Constraints:
- XME + XLE = 2,000 
- XMW + XLW = 930 
- XMN + XLN = 2,200

**Optimised distribution plan**
  
![Distribution Plan](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/2beee3c66148aaa7ba3ec0c3c63ed85b360688f3/Visualisations/Distribution%20Plan.png)
- Linear programming optimised shipping costs to £99,615 by allocating 2,000 units to the East and 500 to the North from Manchester, and 930 units to the West and 1,700 to the North from London. 

### **3.4 Supplier choice based on MAXIMAX, MAXIMIN AND MINIMAX**
Winter Rock faces a dilemma of choosing the optimal supplier to procure mountain skis with bindings that the company wishes to sell at £150. The company has two assumptions: (1) if strong demand is generated, it will be able to sell 1,000 skis, and (2) under low demand conditions, it will be able to sell only 500 skis. Winter Rock needs to decide which supplier to contract. Calculations for gross profit were performed for two suppliers under two different demand scenarios, and a decision tree was made to help visualise the decision process.

![Supplier Choice](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/2beee3c66148aaa7ba3ec0c3c63ed85b360688f3/Visualisations/Supplier%20Choice%20(2).jpg)

**Decision Approaches**

**MAXIMAX (Risk-Seeking) Approach**
- This approach maximises the maximum gross profit that the company could get, assuming it sees high demand.
- Gross profit for EU: -£60,000 (limited to 500 skis due to supply constraints).
- Gross profit for USA: £45,000.
- Assuming Winter Rock is risk-seeking, it should contract the American Supplier to maximise its profitability.
**MAXIMIN (Risk-Aversion) Approach**
- This approach maximises the minimum profit the company could get, assuming demands are low.
- Gross profit for EU: £15,000.
- Gross profit for USA: £20,000.
- Assuming Winter Rock is risk-averse, it should contract the American Supplier to maximise its profitability under low demand conditions.
**MINIMAX Regret Approach**
- This approach evaluates the total loss if the company contracts the less optimal supplier.
- Opportunity loss for EU: £110,000.
- Opportunity loss for USA: £0.
- The USA supplier minimises the regret and is the better choice.
**Final Verdict**
- Contract the American Supplier to maximise profits under both low and high demand scenarios.
### **3.5 Monte-Carlo Simulation to identify profit trends under EU and US supplier**
The stakeholders at Winter Rock are sceptical about the decision analysis. The stakeholders expect the demand to be uniformly distributed between 200 – 800 sales. However, they are not certain if they will be able to sell 500 units of skis. Monte Carlo Simulation was carried out, where 1,000 simulations of sales, assuming uniform distribution, were conducted to assess profitability under two conditions: (1) profitability under EU and (2) profitability under USA.
- **Average Profit:** Profitability for EU supplier shows frequent negative outcomes, with an average loss of £6,688. Profitability for USA supplier is consistently positive, with most values clustering around the average of £19,710.
- **Standard deviation of Profit:** The higher standard deviation (£24,365) for the Europe supplier reflects greater variability in profits, increasing financial risk under uncertainty. The lower standard deviation (£8,648) for the USA supplier demonstrates more stable profitability, reducing risk for Winter Rock.
**Profit probability distribution**
  
  ![Profit - Europe Supplier](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/2beee3c66148aaa7ba3ec0c3c63ed85b360688f3/Visualisations/Profit%20-%20Europe%20Supplier.png)

- The European supplier's lower capacity leads to consistent opportunity losses and higher profit variability, increasing financial risk.
- The European supplier, limited to 500 units, poses higher opportunity losses, with majority of profit probability (69.1%) occurring between profit range of £14.500 and –£34,000.

![Profit - USA Supplier](https://github.com/DipunMohapatra/Winter-Rock-Operations-Optimisation/blob/2beee3c66148aaa7ba3ec0c3c63ed85b360688f3/Visualisations/Profit%20-%20USA%20Supplier.png)

- The USA supplier offers a higher average profit and significantly lower variability, making it the better choice under uncertain demand conditions.
- It is recommended to contract the American supplier, with a 79.1% probability of profits between £22,000 and £10,000
## **4. Final Recommendations**
- To ensure Winter Rock's operational efficiency and profitability, it is recommended that the company increase inventory levels by 10% during the October–January peak season, leveraging historical sales trends for targeted promotional campaigns such as Black Friday and Christmas.
- During off-peak months, focus should shift to cost-effective marketing of high-margin products to sustain profitability. 
- For accurate forecasting, Winter Rock should continue using Simple Exponential Smoothing (SES) with α = 0.55 for short-term predictions while integrating Holt-Winters models for long-term accuracy, accommodating seasonal and trend variations.
- Distribution strategies should adopt the linear programming model, minimising shipping costs to £99,615 by optimally allocating goods from Manchester and London hubs to regional demand centres.
- For the new product launch, the American supplier is recommended due to its scalability and a 79.1% probability of profits between £22,000 and £10,000, as evidenced by Monte Carlo simulations. This should be complemented by risk management strategies to navigate demand variability.
- Finally, Winter Rock should implement an agile monitoring system and invest in advanced analytics tools to track sales trends, forecast accuracy, and inventory dynamics, enabling swift, data-driven decisions to maintain competitiveness and adapt to market changes.












