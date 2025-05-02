# Aiven Oy Financial Performance Analysis (FYE 2020-2024)

This project analyzes the financial performance trends of Aiven Oy, the Finnish company of Aiven, using publicly collected data. The goal is to understand key trends in revenue, profitability, and capital structure for the fiscal years ending March 31, 2020, through March 31, 2024.

## Data

### Source

Data was accessed via publicly available summaries from Finnish business information services (Finder.fi and Asiakastieto.fi).
* **Reporting Period:** Fiscal Years ending March 31st.

### Scope and Limitations

* **Entity Scope:** This analysis focuses strictly on **Aiven Oy**, the legal entity registered in Finland. It does not necessarily represent the consolidated financial performance of the entire global Aiven group, which includes various international subsidiaries.
* **Data Granularity:** The analysis uses publicly available *summary* figures. It does not have access to detailed internal financial statements (e.g., full balance sheet breakdowns, cash flow statements, detailed expense categories).

## Methodology

1.  **Data Collection:** Key financial figures (Revenue, Operating Profit/Loss, Net Profit/Loss, Equity Ratio) were gathered from public summaries of Aiven Oy's PRH filings.
2.  **Data Processing:** Python (`pandas` library) was used to structure the time-series data.
3.  **Ratio Calculation:** Key performance ratios (Operating Profit Margin, Net Profit Margin) were calculated using pandas.
4.  **Data Visualization:** Python libraries (`matplotlib`, `seaborn`) were used to generate charts visualizing trends over the five fiscal years.


## Key Metrics Analyzed

* Revenue (€ Millions)
* Operating Profit/Loss (€ Millions)
* Net Profit/Loss (€ Millions)
* Operating Profit Margin (%)
* Net Profit Margin (%)
* Equity Ratio (%)

## Key Findings

* **Strong Revenue Growth:** Aiven Oy demonstrated significant revenue growth over the five-year period analyzed (FYE 2020-2024).
* **Persistent Losses:** The company reported consistent operating and net losses during this high-growth phase, which is common for venture-backed scale-ups investing heavily in expansion.
* **Improving Margins (Recent):** While still negative, the Net Profit Margin showed a marked improvement in the latest reported year (2024: -31.4%) compared to the previous year (-60.3%).
* **Low Leverage:** The Equity Ratio remained consistently high (above 90%), indicating a strong reliance on equity financing (venture capital) rather than traditional debt, reflecting its funding structure.

## Visualizations

![aiven](https://github.com/user-attachments/assets/09828b38-4a49-4865-8648-c4eba78a5f17)


## Technology Stack & Code

* Python 3
* Pandas
* Matplotlib
* Seaborn
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.ticker as mtick 

data_financials = {
    'FiscalYearEnd': pd.to_datetime(['2020-03-31', '2021-03-31', '2022-03-31', '2023-03-31', '2024-03-31']),
    'Revenue_M_EUR': [8.37, 16.69, 37.66, 65.12, 74.63],
    'OperatingProfitLoss_M_EUR': [-1.27, -6.61, -13.04, -43.99, -34.09],
    'NetProfitLoss_M_EUR': [-1.06, -6.31, -8.83, -39.27, -23.45],
    'EquityRatio_Percent': [92.0, 92.0, 91.0, 94.0, 92.0]
}
df_ratios = pd.DataFrame(data_financials)
df_ratios.set_index('FiscalYearEnd', inplace=True)
df_ratios['OperatingProfitMargin_Percent'] = (df_ratios['OperatingProfitLoss_M_EUR'] / df_ratios['Revenue_M_EUR']) * 100
df_ratios['NetProfitMargin_Percent'] = (df_ratios['NetProfitLoss_M_EUR'] / df_ratios['Revenue_M_EUR']) * 100


# Plot
sns.set_style("whitegrid")

# Create subplots
fig, axes = plt.subplots(2, 2, figsize=(14, 10)) 
fig.suptitle('Aiven Oy Financial Performance Trends (FYE March 2020-2024)', fontsize=16)

years = df_ratios.index.year

# Chart 1: Revenue Trend
axes[0, 0].plot(df_ratios.index, df_ratios['Revenue_M_EUR'], marker='o', linestyle='-', color='blue')
axes[0, 0].set_title('Revenue Growth')
axes[0, 0].set_ylabel('Revenue (€ Millions)')
axes[0, 0].set_xticks(df_ratios.index) 
axes[0, 0].set_xticklabels(years) 
axes[0, 0].grid(True)

# Chart 2: Operating & Net Profit/Loss Trend 
axes[0, 1].plot(df_ratios.index, df_ratios['OperatingProfitLoss_M_EUR'], marker='s', linestyle='-', label='Operating Profit/Loss')
axes[0, 1].plot(df_ratios.index, df_ratios['NetProfitLoss_M_EUR'], marker='^', linestyle='--', label='Net Profit/Loss')
axes[0, 1].set_title('Profitability (Absolute)')
axes[0, 1].set_ylabel('Amount (€ Millions)')
axes[0, 1].set_xticks(df_ratios.index) 
axes[0, 1].set_xticklabels(years) 
axes[0, 1].legend()
axes[0, 1].grid(True)
axes[0, 1].axhline(0, color='black', linewidth=0.5, linestyle='--') 

# Chart 3: Operating & Net Profit Margin Trend
axes[1, 0].plot(df_ratios.index, df_ratios['OperatingProfitMargin_Percent'], marker='o', linestyle='-', label='Operating Profit Margin')
axes[1, 0].plot(df_ratios.index, df_ratios['NetProfitMargin_Percent'], marker='s', linestyle='--', label='Net Profit Margin')
axes[1, 0].set_title('Profitability Margins')
axes[1, 0].set_ylabel('Margin (%)')
axes[1, 0].set_xlabel('Fiscal Year End')
axes[1, 0].set_xticks(df_ratios.index) 
axes[1, 0].set_xticklabels(years) 
axes[1, 0].yaxis.set_major_formatter(mtick.PercentFormatter()) 
axes[1, 0].legend()
axes[1, 0].grid(True)
axes[1, 0].axhline(0, color='black', linewidth=0.5, linestyle='--') 

# Chart 4: Equity Ratio Trend
axes[1, 1].plot(df_ratios.index, df_ratios['EquityRatio_Percent'], marker='^', linestyle='-', color='green')
axes[1, 1].set_title('Equity Ratio (Leverage Proxy)')
axes[1, 1].set_ylabel('Equity Ratio (%)')
axes[1, 1].set_xlabel('Fiscal Year End')
axes[1, 1].set_xticks(df_ratios.index) 
axes[1, 1].set_xticklabels(years) 
axes[1, 1].set_ylim(bottom=85, top=100) 
axes[1, 1].yaxis.set_major_formatter(mtick.PercentFormatter(xmax=100.0)) 
axes[1, 1].grid(True)

# plot
plt.tight_layout(rect=[0, 0.03, 1, 0.95])
plt.show()
```
