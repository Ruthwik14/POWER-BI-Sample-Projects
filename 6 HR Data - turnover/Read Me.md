# 💼 HR Turnover Analysis Dashboard

## 📌 Project Overview

This project focuses on delivering critical insights into **employee turnover** within a company. The core business problem addressed was the need for HR and leadership to understand hiring and attrition trends, identify high-turnover areas, and make data-driven decisions to improve **employee retention**. To achieve this, I developed an **interactive Power BI dashboard** using historical HR data, enabling **proactive workforce management** and supporting sustainable organizational growth.

## 🗂️ Data Description

The analysis is based on an Excel file containing two main datasets: "New Joinees" and "Leavers."

* **Type**: Historical HR data, including employee demographics and employment dates.

* **Source**: A public Power BI practice dataset from [Chandoo.org](https://chandoo.org/).

* **Key Columns**:

  * **Employee Demographics**: `Name`, `Gender`, `Department`, `Branch`

  * **Employment Dates**: `Date of Joining`, `Date of Leaving`

  * **Job Role**: `Designation`

* **Date Range Covered**: January 1, 2018 – February 28, 2019.

## 🧭 Methodology / Approach

My approach involved a systematic process of data handling, metric calculation, and visualization:

### 1️⃣ Data Acquisition & Preparation

* I merged the "New Joinees" and "Leavers" datasets using employee names to create a unified view.

* A dedicated **Calendar Table** was created to ensure accurate time-based analysis and enable robust trend tracking.

* I established clear data model relationships within Power BI to connect the different data points effectively.

### 2️⃣ Key Metric Calculation

I calculated essential HR Key Performance Indicators (KPIs) to measure workforce dynamics:

* **Joinee Count**: The total number of new hires.

* **Leaver Count**: The total number of employee exits.

* **Total Staff to Date**: A running total of active staff at any given point.

* **Turnover %**: Calculated as `(Leaver Count / Joinee Count)`, this metric highlights the rate at which employees are leaving the company relative to new hires.

### 3️⃣ Interactive Dashboard Development

I built a **single-page, interactive Power BI dashboard** designed for ease of use by HR teams and senior leadership. Key visual elements include:

* **Department Slicer**: Allows users to filter data by specific departments.

* **Line Charts**: Display monthly trends for both Joinee Count and Leaver Count over time.

* **Forecast Chart**: Provides a 6-month forecast of the Total Staff Count, aiding in future planning.

* **Stacked Bar Chart**: Illustrates Leaver Count broken down by gender and branch, revealing specific areas of attrition.

* **Top 10 Table**: Ranks designations by their Turnover %, identifying roles with the highest attrition rates.

* **Multi-Row Cards**: Provide quick summaries of Joinee Count, Leaver Count, and Total Staff to Date across different timeframes (full timeline, last 3 months, and last 1 month).

## 📈 Key Results and Insights

The dashboard provides actionable insights into the company's workforce:

* **🔄 Employee Movement Trends**: Clearly visualizes hiring and attrition patterns over time, highlighting any seasonality or spikes in staff movement to aid in resource planning.

* **🚨 High-Turnover Areas**: Pinpoints specific departments, branches, and job roles experiencing high attrition, enabling targeted root cause analysis and intervention.

* **📉 Workforce Growth/Contraction**: Tracks changes in the total staff count, offering a historical perspective and a short-term forecast to support budget and recruitment planning.

* **🚻 Gender & Location-Based Turnover**: Analyzes attrition rates based on gender and branch, revealing potential disparities that can inform Diversity & Inclusion (D&I) initiatives and policy improvements.

*A snapshot of the dashboard is included in the project files for reference.*

## 🛠️ Tools & Technologies Used

* **Power BI**: Utilized for data modeling, creating DAX measures, and developing the interactive dashboard.

* **Power Query**: Employed for robust data cleaning, merging, and transformation processes (ETL).

* **Microsoft Excel**: Used as the primary data storage format for the raw datasets.

## 💡 Business Impact

This project empowers HR and leadership to make more informed decisions, leading to a more stable and productive workforce:

* ✅ **Improved Retention Strategies**: By identifying and addressing the root causes of attrition, the company can develop more effective retention programs.

* 🔍 **Optimized Recruitment**: Insights into high-turnover roles and departments allow for more focused and efficient hiring efforts.

* 📅 **Enhanced Workforce Planning**: Accurate forecasting and trend analysis facilitate better staffing decisions for future business needs.

* 📊 **Driven HR Efficiency**: The interactive dashboard provides self-service, real-time reporting, reducing manual effort and speeding up access to critical information.

* 🎯 **Supported Strategic Decisions**: Leadership gains data-backed insights to guide overall business strategy and resource allocation.

**Outcome:** This project contributes to a more **stable, productive, and cost-effective** workforce, ultimately enhancing overall business performance.

## Contact Information / About Me 📧

**Devanapalli Ruthwik**
**Senior Data Analyst | Business Consultant**

* **Email**: ruthwik.devanapalli@gmail.com

* **LinkedIn**: <https://www.linkedin.com/in/ruthwik-devanapalli/>

This project is intended for educational, portfolio, and demonstration purposes. Attribution is appreciated when used or adapted.