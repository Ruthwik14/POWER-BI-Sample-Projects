# 🌎📊 COVID-19 World Vaccination Progress Analysis

## 📈 Project Overview

This project focuses on analyzing **global COVID-19 vaccination data from 2020 to March 2022** to provide actionable insights into vaccination trends and progress worldwide. The primary goal was to transform raw vaccination data into clear, interactive visualizations, identify top-performing countries, and observe vaccination patterns over time. This analysis empowers public health officials, data scientists, and policy makers to quickly grasp the global vaccination landscape and make informed decisions.

## 📁 Data Description

The analysis utilized a comprehensive dataset detailing country-level COVID-19 vaccination progress.

* **Type**: Country-level COVID-19 vaccination data.

* **Source**: Kaggle - COVID World Vaccination Progress dataset.

* **Data Size**: Records spanning from **2020 to March 2022**.

* **Key Attributes**:

  * `Country` and `Country ISO Code`: Geographical identifiers.

  * `Date`: Date of data entry.

  * `Total number of vaccinations`: Cumulative vaccinations in a country.

  * `Number of people vaccinated`: Individuals who received at least one dose.

  * `Total number of people fully vaccinated`: Individuals who completed their immunization scheme.

  * `Daily vaccinations (raw and per million)`: Daily new vaccinations, both absolute and per capita.

  * `Vaccines used in the country`: List of vaccines administered.

  * `Source name` and `Source website`: Origin of the information.

## 🛠️ Methodology / Approach

A structured, end-to-end approach was adopted to ensure data integrity and impactful storytelling:

### 📥 Data Acquisition

* The dataset was sourced as a CSV file from Kaggle.

### 🔄 Data Loading & Transformation

* The raw data was loaded into **Power BI Desktop**.

* Data cleaning and transformation steps were performed, including handling missing values and converting data types (e.g., dates and numerical values) to ensure accuracy and usability.

### 🔗 Data Modeling

* Relationships were established between tables (if multiple were used) to enable integrated analysis and accurate calculations across different data dimensions.

### 📊 Interactive Dashboard Development

* A dynamic **Power BI dashboard** was built, incorporating various visual elements to present insights effectively:

  * **Card visuals**: Display key aggregate metrics (e.g., total vaccinations in India).

  * **Clustered charts**: Used to compare metrics across different countries or categories (e.g., top 10 fully vaccinated countries).

  * **Grayscale map visuals**: Visually represent total vaccinations geographically with bubble overlays, where bubble size indicates the volume of vaccinations.

  * **Custom slicers**: Implemented for easy filtering by Year and Month, allowing users to explore trends over specific periods.

### 🔍 Insight Generation

* Key observations and trends were generated directly from the interactive dashboard, providing a clear understanding of vaccination progress globally and in specific regions.

## ✨ Key Results

The Power BI dashboard uncovers the following critical insights:

* **🇮🇳 India’s Vaccination Status**:

  * Dedicated card visuals prominently display India’s **Total Vaccinations**, **People Vaccinated**, and **People Fully Vaccinated**, with a flexible date slicer for year and month navigation.

* **🌐 Top Fully Vaccinated Countries (2021)**:

  * A clustered column chart clearly highlights the **top 10 countries with the highest number of fully vaccinated people in 2021**, identifying leaders in immunization efforts.

* **🗺️ Global Vaccination Overview**:

  * A grayscale map visually represents the **total vaccinations for all countries** using bubble sizes, providing an intuitive global overview of vaccination scale.

* **📅 Daily Vaccination Trends**:

  * A clustered bar chart shows the **top 10 countries with the highest number of daily vaccinations**, with a date slicer for year and month. This reveals significant vaccination drives and patterns in **2020 and 2021**.

* **📈 India’s Fully Vaccinated Trend**:

  * A line chart illustrates the **cumulative count of fully vaccinated people in India over time**, showcasing the ongoing progress of the national immunization program.

## ⚙️ Tools and Technologies Used

* **Power BI Desktop**: Utilized for data ingestion, transformation, data modeling, and designing the interactive dashboard.

* **CSV Files**: The format of the raw dataset used for analysis.

## 💰 Business Impact / Why This Project Is Valuable

This project brings immense value to public health organizations, data science teams, and policy makers by:

* ✅ **Enabling Informed Decision-Making**: Offers real-time insights into global and national vaccination progress, critical for resource allocation and public health strategies.

* 📉 **Identifying Key Trends**: Highlights countries leading vaccination efforts and critical timelines of progress, allowing for benchmarking and best practice identification.

* 🧑‍💼 **Stakeholder Communication**: Transforms complex global data into intuitive and visual formats, making it accessible and understandable for both technical and non-technical stakeholders.

* 🎯 **Demonstrating Analytical Proficiency**: Showcases the ability to handle real-world, large-scale data, extract meaningful value, and communicate results effectively through interactive dashboards.

## Contact Information / About Me 📧

**Devanapalli Ruthwik**
**Senior Data Analyst | Business Consultant**

* **Email**: ruthwik.devanapalli@gmail.com

* **LinkedIn**: <https://www.linkedin.com/in/ruthwik-devanapalli/>

This project is intended for educational, portfolio, and demonstration purposes. Attribution is appreciated when used or adapted.