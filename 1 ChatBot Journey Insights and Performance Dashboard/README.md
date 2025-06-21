# 💬 Chatbot Journey Insights and Performance Dashboard

## 🚀 Project Overview

This project addresses the critical need for efficient and insightful management of **Yojana promotion in election campaigning**. With citizen inquiries originating from chatbots and missed calls across three different languages, and then being directed to specialized call centers for resolution, there was a challenge in gaining a unified view of the citizen journey and agent performance.

The **Chatbot Journey Insights and Performance Dashboard** was developed to provide a comprehensive, real-time overview of the entire citizen interaction process. This dashboard visually maps out the paths citizens take when seeking assistance, from initial chatbot interactions and missed calls to the eventual resolution by specialized call centers. It allows for the identification of bottlenecks, understanding citizen needs, and evaluating the effectiveness of various support channels and call centers, ultimately improving service delivery for individuals seeking assistance with Yojana (government scheme) related queries.

## 📊 Data Description

The project utilizes a rich dataset encompassing citizen interactions from multiple sources, as shown in the provided data schemas:

* **Chatbot Data **: Records of conversations initiated through the chatbot, available in English, French, and German. Key fields include \`Age Group\`, \`Call center\`, \`Canton\`, \`Chat Date\`, \`Chat Time\`, \`District\`, \`Gender\`, \`Issue\`, \`Language\`, \`Mobile Number\`, \`Name\`, \`Scheme Status\`, and \`Scheme\`.

* **Missed Call Data **: Information on calls that were initially missed and then routed through the chatbot or directly to call centers. Key fields include \`CALL STATUS\`, \`CALLER\`, \`DATE\`, \`DayName\`, \`DURATION\`, \`PULSE COUNT\`, \`TIME\`, \`TIME (bins)\`, and \`VIRTUAL NUM\`.

* **Our Call Center Data **: Records from the primary call center detailing resolution status, call types, and caller information. Key fields include \`Age Group\`, \`Agent Name\`, \`Call Date\`, \`Call Time\`, \`Call Type\`, \`Canton\`, \`Chat Date\`, \`Data From\`, \`Dialer Comments\`, \`Dialer Status\`, \`District\`, \`Duration\`, \`Gender\`, \`Issue\`, \`Language\`, \`Mobile Number\`, and \`Name\`.

* **Division Call Center Data **: Data from the District Office Call Center. Key fields include \`Age Group\`, \`Agent Name\`, \`Call Date\`, \`Call Time\`, \`Call Type\`, \`Canton\`, \`Chat Date\`, \`Chat Time\`, \`Dialer Comments\`, \`Dialer Status\`, \`District\`, \`Duration\`, \`Gender\`, \`Issue\`, \`Language\`, \`Mobile Number\`, and \`Question\`.

* **MLA Call Center Data **: Data from the National Councillor Office Call Center. Key fields include \`Age Group\`, \`Agent Name\`, \`Call Date\`, \`Call Time\`, \`Call Type\`, \`Canton\`, \`Chat Date\`, \`Chat Time\`, \`Dialer Comments\`, \`Dialer Status\`, \`District\`, \`Duration\`, \`Gender\`, \`Issue\`, \`Language\`, \`Mobile Number\`, and \`Name\`.

* **Bridge & Common Data **: Auxiliary tables for bridging phone numbers and identifying common mobile numbers between chatbot and missed calls, along with various calculated measures and filters.

**Data Size:** The data is substantial, with the chatbot data alone showing over **2.3 million total chats**, and missed call data indicating nearly **1 million total missed calls**.

## 🧠 Methodology / Approach

The development of this dashboard followed a structured data analysis and visualization approach:

### 🌐 1. Data Ingestion & Translation

* Collected raw data from various sources including English, French, and German chatbot logs, missed call data, and call center records (Our Call Center, National Councillor Office, District Office).

* Implemented an automated translation process using \`deep_translator\` (specifically \`GoogleTranslator\`) to convert non-English text in key columns (Language, Canton, District, Gender, Scheme, Issue) across multiple sheets into English. This ensured consistency and enabled unified analysis.

* Leveraged \`ThreadPoolExecutor\` for parallel processing during translation to enhance efficiency.

### 🧹 2. Data Cleaning & Standardization

This section details the automated data cleaning and standardization steps performed by the **Python script** (`chat bot daily cleaning file.ipynb`) before data is loaded into Power BI. These steps ensure data quality and consistency, which are crucial prerequisites for effective data modeling and DAX calculations.

* **Handling Translated Columns:** After the initial translation process, the Python script systematically drops the original language columns (e.g., 'Langue', 'Sexe') and any intermediate `_translated` columns. This (`df_sheet1 = df_sheet1.drop(columns=['Language_translated',...],errors='ignore')`) ensures that only the standardized English versions of the data are retained, removing redundancy and improving clarity.

* **Column Renaming:** The script consistently renames key columns across all datasets to establish a uniform naming convention. For instance, 'Phone' is renamed to 'Mobile Number' (`df_sheet1.rename(columns={'Phone': 'Mobile Number'}, inplace=True)`), and similar renames are applied to French ('Téléphone' to 'Mobile Number', 'Nom' to 'Name') and German ('Telefonnummer' to 'Mobile Number') datasets.

* **Merging with Lookup Tables for Standardization:** To ensure consistent categorization and address potential variations or missing values (`NaN`) in critical categorical fields, the Python script merges the dataframes with external Excel lookup files. This process effectively standardizes values for categories such as `Language`, `Gender`, `Scheme`, `Issue`, `District`, and `Canton`. This merging ensures that all related terms are mapped to a single, predefined English term, which is vital for accurate aggregation and filtering in the Power BI dashboard.

* **Reordering Columns:** The script reorders columns into a logical and consistent sequence across all datasets. This improves readability and streamlines the data structure for subsequent analysis and data modeling in Power BI.

### 🗄️ 3. Data Modeling & Transformation

* Combined the cleaned chatbot, missed call, and various call center datasets to create a holistic view of the citizen journey.

* Derived key metrics and calculated fields necessary for performance analysis (e.g., total chats, chats transferred, call resolution rates by different call centers).

* **Implemented dynamic "Total Count" vs. "Unique Count" measures using DAX functions**. This allows the user to switch between viewing all interactions (Total Count) and unique citizen interactions (Unique Count) across various metrics like chats, missed calls, and call center activities. This provides a versatile analytical capability for different business perspectives. **Key DAX functions used include \`CALCULATE\`, \`COUNTROWS\`, \`DISTINCTCOUNT\`, \`FILTER\`, \`SELECTEDVALUE\`, \`SWITCH\`, \`UNION\`, \`INTERSECT\`, \`VALUES\`, and \`SELECTCOLUMNS\`.**

#### Key DAX Measures and Functions Explained

The following DAX measures were crucial for enabling the dynamic filtering and comprehensive analysis within the dashboard. Each measure is designed to provide a specific analytical perspective, often leveraging the "Total Count" vs. "Unique Count" selection:

* **`BridgePhoneNumbers`**:
  * **Purpose:** Creates a consolidated, distinct list of all phone numbers present across both the 'Chatbot Data' and 'Missed call Data' tables. This is foundational for linking related interactions from different channels.
  * **DAX Functions:** `DISTINCT`, `UNION`, `SELECTCOLUMNS`.
  * **Significance:** Ensures a comprehensive bridge table for cross-channel analysis, allowing for the identification of common citizens interacting via multiple platforms.

* **`Common_mob_between_chatbot_missed_call`**:
  * **Purpose:** Identifies the mobile numbers that are common to both 'Chatbot Data' and 'Missed call Data'. This highlights citizens who have interacted through both channels.
  * **DAX Functions:** `INTERSECT`, `VALUES`.
  * **Significance:** Crucial for understanding multi-channel engagement and for segmenting citizens who use different support methods.

* **`FilterChatbotCount`, `FilterMissedCallCount`, `FilterMLACalldone`, `FilterDivisionCalldone`, `Filtermissedcalldone`, `Filterourcallcenter`**:
  * **Purpose:** These are control measures that dynamically switch between "Total Count" and "Unique Count" for various interaction metrics (e.g., total chats, missed calls, calls handled by specific centers).
  * **DAX Functions:** `VAR`, `SELECTEDVALUE`, `RETURN`, `SWITCH`, `TRUE()`.
  * **Significance:** Provides a highly interactive user experience, allowing stakeholders to choose whether they want to see the sheer volume of interactions or the number of unique citizens involved, offering different analytical depths.

* **`Totalchats` (`COUNTROWS('1 Chatbot Data')`)**:
  * **Purpose:** Calculates the total number of rows in the '1 Chatbot Data' table, representing the absolute count of all chatbot interactions.
  * **DAX Function:** `COUNTROWS`.
  * **Significance:** Provides the raw volume of chatbot activity.

* **`UniqueChats` (`DISTINCTCOUNT('1 Chatbot Data'[Mobile Number])`)**:
  * **Purpose:** Calculates the number of distinct mobile numbers in the '1 Chatbot Data' table, representing the unique citizens who interacted via chatbot.
  * **DAX Function:** `DISTINCTCOUNT`.
  * **Significance:** Helps understand the reach of the chatbot in terms of individual citizens served.

* **`TotalMissedCallCount` (`COUNTROWS('2 Missed call Data'), REMOVEFILTERS('1 Chatbot Data')`)**:
  * **Purpose:** Calculates the total number of missed call records. `REMOVEFILTERS` ensures that cross-filters from the Chatbot data don't affect this total, providing a standalone count.
  * **DAX Functions:** `CALCULATE`, `COUNTROWS`, `REMOVEFILTERS`.
  * **Significance:** Provides the overall volume of missed calls before any transfers.

* **`UniqueMissedCallCount` (`DISTINCTCOUNT('2 Missed call Data'[CALLER]), REMOVEFILTERS('1 Chatbot Data')`)**:
  * **Purpose:** Calculates the count of unique callers in the 'Missed call Data' table, independent of any filters from the Chatbot data.
  * **DAX Functions:** `CALCULATE`, `DISTINCTCOUNT`, `REMOVEFILTERS`.
  * **Significance:** Identifies the number of distinct citizens who experienced a missed call.

* **`Total_call_tranferes_to_callcenter`**:
  * **Purpose:** Calculates the total number of calls transferred to call centers by subtracting the common chatbot/missed call interactions from the total missed call count.
  * **DAX Function:** Basic arithmetic operation on other measures.
  * **Significance:** Quantifies the hand-off volume from automated systems to human agents.

* **`TotalMLACount`, `distintMLACount`**:
  * **Purpose:** Calculate the total count and distinct count of interactions for the MLA (National Councillor Office) Call Center, specifically based on mobile numbers.
  * **DAX Functions:** `CALCULATE`, `COUNT`, `DISTINCTCOUNT`.
  * **Significance:** Measures the total volume and unique citizen interactions handled by the MLA center.

* **`TotalDivisionCount`, `distinctDivisionCount`**:
  * **Purpose:** Calculate the total count (based on District) and distinct count of interactions (based on Mobile Number) for the Division (District Office) Call Center.
  * **DAX Functions:** `CALCULATE`, `COUNT`, `DISTINCTCOUNT`.
  * **Significance:** Provides insights into the workload and reach of the District Office.

* **`TotalCallsPerCategory`, `uniqueCallsDone` (and variations like `Totalmissedcalldone`, `uniquemissedcalldone`)**:
  * **Purpose:** These measures are used to calculate total and unique interactions handled by 'Our Call Center' and specifically for 'missed call' data processed by 'Our Call Center'.
  * **DAX Functions:** `CALCULATE`, `COUNTROWS`, `DISTINCTCOUNT`, `FILTER`, `ALL`.
  * **Significance:** Helps in assessing the performance of the primary call center and its effectiveness in addressing different types of inquiries, including those originating as missed calls.

* **`Intersecting TotalCountChatbot & MissedCall` & `intersecting DistintCountChatbot&missedcall`**:
  * **Purpose:** Calculate the total and distinct count of interactions where a mobile number from the 'Missed call Data' is also found in the 'Chatbot Data' based on a common mobile number bridge.
  * **DAX Functions:** `CALCULATE`, `COUNTROWS`, `DISTINCTCOUNT`, `FILTER`, `IN`, `VALUES`.
  * **Significance:** Quantifies the overlap between chatbot and missed call interactions, indicating citizens who use both channels.

* **`Measures and filters` (DAX `DATATABLE`)**:
  * **Purpose:** Creates a virtual table used for the slicer that allows users to select "Total Count" or "Unique Count".
  * **DAX Function:** `DATATABLE`.
  * **Significance:** Provides the underlying data structure for the interactive "Unique vs. Total Count Filter".

```dax
6 BridgePhoneNumbers =
DISTINCT (
UNION (
SELECTCOLUMNS('2 Missed call Data', "CALLER", '2 Missed call Data'\[CALLER\]),
SELECTCOLUMNS('1 Chatbot Data', "Mobile Number", '1 Chatbot Data'\[Mobile Number\])
)
)

7 Common_mob_between_chatbot_missed_call = INTERSECT(
VALUES('1 Chatbot Data'\[Mobile Number\]),
VALUES('2 Missed call Data'\[CALLER\])
)

1 FilterChatbotCount =
VAR SelectedValue = SELECTEDVALUE('8 Measures and filters'\[Type\], "Total Count")
RETURN
SWITCH(
TRUE(),
SelectedValue = "Total Count", \[2 Totalchats\],
SelectedValue = "Unique Count", \[3 UniqueChats\]
)

10 Total_call_tranferes_to_callcenter = \[4 FilterMissedCallCount\] - \[7 FilterintersectingChatbotand_missedcall\]

11 FilterMLACalldone =
VAR SelectedValue = SELECTEDVALUE('8 Measures and filters'\[Type\], "Total Count")
RETURN
SWITCH(
TRUE(),
SelectedValue = "Total Count", \[12 TotalMLACount\],
SelectedValue = "Unique Count", \[13 distintMLACount\]
)

12 TotalMLACount =
CALCULATE(
COUNT('5 MLA'\[Mobile Number\])
)

13 distintMLACount =
CALCULATE(
DISTINCTCOUNT('5 MLA'\[Mobile Number\]
))

14 FilterDivisionCalldone =
VAR SelectedValue = SELECTEDVALUE('8 Measures and filters'\[Type\], "Total Count")
RETURN
SWITCH(
TRUE(),
SelectedValue = "Total Count", \[15 TotalDivisionCount\],
SelectedValue = "Unique Count", \[16 distinctDivisionCount\]
)

15 TotalDivisionCount =
CALCULATE(
COUNT('4 Division'\[District\]
))

16 distinctDivisionCount =
CALCULATE(
DISTINCTCOUNT('4 Division'\[Mobile Number\])
)

17 Filtermissedcalldone =
VAR SelectedValue = SELECTEDVALUE('8 Measures and filters'\[Type\], "Total Count")
RETURN
SWITCH(
TRUE(),
SelectedValue = "Total Count", \[18 TotalCallsPerCategory\],
SelectedValue = "Unique Count", \[19 uniqueCallsDone\]
)

18 TotalCallsPerCategory =
CALCULATE(
COUNTROWS('3 Our Call Center Data')
)

19 uniqueCallsDone = CALCULATE(
DISTINCTCOUNT('3 Our Call Center Data'\[Mobile Number\])
)

2 Totalchats = COUNTROWS('1 Chatbot Data')

20 Filterourcallcenter =
VAR SelectedValue = SELECTEDVALUE('8 Measures and filters'\[Type\], "Total Count")
RETURN
SWITCH(
TRUE(),
SelectedValue = "Total Count", \[18 TotalCallsPerCategory\],
SelectedValue = "Unique Count", \[19 uniqueCallsDone\]
)

21 filtermissedcalldone =
VAR SelectedValue = SELECTEDVALUE('8 Measures and filters'\[Type\], "Total Count")
RETURN
SWITCH(
TRUE(),
SelectedValue = "Total Count", \[22 Totalmissedcalldone\],
SelectedValue = "Unique Count", \[23 uniquemissedcalldone\]
)

22 Totalmissedcalldone =
CALCULATE(
COUNTROWS('3 Our Call Center Data')
)

23 uniquemissedcalldone =
CALCULATE(
DISTINCTCOUNT('3 Our Call Center Data'\[Mobile Number\])
)

3 UniqueChats = DISTINCTCOUNT('1 Chatbot Data'\[Mobile Number\])

4 FilterMissedCallCount =
VAR SelectedValue = SELECTEDVALUE('8 Measures and filters'\[Type\], "Total Count")
RETURN
SWITCH(
TRUE(),
SelectedValue = "Total Count", \[5 TotalMissedCallCount\],
SelectedValue = "Unique Count", \[6 UniqueMissedCallCount\]
)

5 TotalMissedCallCount = CALCULATE(
COUNTROWS('2 Missed call Data'),
REMOVEFILTERS('1 Chatbot Data')
)

6 UniqueMissedCallCount = CALCULATE(
DISTINCTCOUNT('2 Missed call Data'\[CALLER\]),
REMOVEFILTERS('1 Chatbot Data')
)

7 FilterintersectingChatbotand_missedcall =
VAR SelectedValue = SELECTEDVALUE('8 Measures and filters'\[Type\], "Total Count")
RETURN
SWITCH(
TRUE(),
SelectedValue = "Total Count", \[8 Intersecting TotalCountChatbot & MissedCall\],
SelectedValue = "Unique Count", \[9 intersecting DistintCountChatbot&missedcall\]
)

8 Intersecting TotalCountChatbot & MissedCall =
CALCULATE(
COUNTROWS('2 Missed call Data'),
FILTER(
'1 Chatbot Data',
'1 Chatbot Data'\[Mobile Number\] IN VALUES('7 Common_mob_between_chatbot_missed_call'\[Mobile Number\])
)
)

9 intersecting DistintCountChatbot&missedcall = CALCULATE(
DISTINCTCOUNT('2 Missed call Data'\[CALLER\]),
'1 Chatbot Data'\[Mobile Number\] IN VALUES('7 Common_mob_between_chatbot_missed_call'\[Mobile Number\])
)

Totalmiisedcallcount = COUNTROWS('2 Missed call Data')

8 Measures and filters =
DATATABLE(
"type", STRING,
{
{"Unique Count"},
{"Total Count"}
}
)

uniqueMissedCallsDone = CALCULATE(
DISTINCTCOUNT('3 Our Call Center Data'\[Mobile Number\]),
'3 Our Call Center Data'\[Name\] = "missed call",
ALL('1 Chatbot Data')
)
```

### 📊 4. Dashboard Development & Visualization

* Designed and developed an interactive dashboard to visualize key performance indicators and trends using Power BI.

* Created various charts and graphs to represent data effectively, such as:

  * Total Chat Count with Respect To Dates/Time

  * Total Missed Call Count with Respect To Dates/Time/Days

  * Call Resolution Status by various call centers (Our Call Center, National Councillor Office, District Office)

  * Top districts by call/chat count

  * Call/chat count by language, age group, gender, and Yojana scheme.

  * Top issues by scheme, top questions by count, top callers by count.

  * Call count by dialer status and call type.

* Implemented interactive filters for Date, District, **Unique vs. Total Count, Scheme, and Issue Resolved to allow users to explore specific data segments.

#### Dashboard Page-by-Page Explanation

The Power BI dashboard is structured into several interconnected pages, each designed to provide specific insights into the citizen journey and call center performance.

**Page 1: Chatbot Journey Dashboard (Overview)**

* **Purpose:** Provides a high-level overview of the entire citizen interaction flow, from initial contact points (chats and missed calls) to the final call center resolution status.

* **Filters:**

  * **Date Filter:** Allows selection of a specific date range for analysis (e.g., 10/10/2024 - 1/31/2025).

  * **District Filter:** Filters data by geographical district.

  * **Unique vs. Total Count Filter:** A crucial slicer (powered by DAX measures like `FilterChatbotCount`) that enables users to toggle between viewing the total number of interactions or the number of unique citizens involved.

* **Key Visuals & Insights:**

  * **Total Chat's (Card):** Displays the total volume of chatbot interactions (e.g., 2,346,117).

  * **Total Missed Call's (Card):** Shows the total number of missed calls (e.g., 985,623).

  * **Flow Arrows & Cards:** Visually represents the journey:

    * "Total missed call transferred to Chatbot" (e.g., 673,332).

    * "Total Missed Call's transferred to Call Center" (e.g., 312,291).

    * "Chats transferred to Call center" (e.g., 986,767).

    * Breakdown of calls done by specific centers: "Call done by National Councillor Office Call Center (semi critical)" (e.g., 151,778), "Call done by District Office Call Center (Critical)" (e.g., 124,518), "Call done by Our Call Center (Chat bot data + Missed call data)" (e.g., 234,574).

  * **Call Resolution Status (Bar Charts):** Shows the breakdown of resolution statuses (Resolution Provided, Follow-up or Escalation Needed, Blank) for National Councillor Office, District Office, and Our Call Center. This immediately highlights resolution efficiency for each center.

**Page 2: Chatbot Dashboard (Detailed Chat Insights)**

* **Purpose:** Provides a deeper dive into chatbot interaction patterns and citizen demographics.

* **Filters:** Same as Page 1, plus:

  * **Scheme Filter:** Filters data based on the Yojana scheme.

  * **Issue Resolved Filter:** Filters by the resolution status of the issue.

* **Key Visuals & Insights:**

  * **Total Chat Count With Respect To Dates (Line Chart):** Shows the trend of chatbot interactions over time (e.g., daily/monthly trends in Dec 2024 - Jan 2025).

  * **Total Chat Count With Respect To Language (Bar Chart):** Displays chat volume by language (e.g., Germany (1.62M), French (398K), English (207K)), indicating language preferences.

  * **Total Chat Count With Respect To Age (Bar Chart):** Shows chat volume distribution across different age groups (e.g., 35-40 age group having the highest interactions).

  * **Total Chat Count With Respect To Scheme Assistance (Bar Chart):** Highlights the most frequently queried Yojana schemes/issues (e.g., Pending payment, Help applying, Benefit issue).

  * **Top/Bottom District by Chat Count (Bar Charts):** Ranks districts by chat volume (e.g., Basel-Stadt as top, Küssnacht as bottom), useful for geographical targeting.

  * **Total Chat Count With Respect To Scheme (Bar Chart):** Similar to scheme assistance, but might show overall scheme inquiries.

  * **Gender (Donut Chart):** Shows the gender distribution of chatbot users.

**Page 3: Missed Call Dashboard**

* **Purpose:** Focuses specifically on missed call data, providing insights into their volume, temporal patterns, and transfer status.

* **Filters:** Date Filter, Unique vs. Total Count Filter.

* **Key Visuals & Insights:**

  * **Total Missed Call's (Card):** Confirms the total missed call volume (e.g., 985,623).

  * **Total Call's transferred to Call Center (Card):** Shows calls transferred to call centers (e.g., 673,332).

  * **Total Missed Call Count With Respect To Dates (Line Chart):** Trends of missed calls over time.

  * **Total Missed Call Count With Respect To Time (Bar Chart):** Hourly patterns of missed calls.

  * **Total Missed Call Count With Respect To Days (Bar Chart):** Daily patterns of missed calls.

  * **Total missed call transferred to Chatbot (Card):** Shows calls transferred to chatbot (e.g., 312,291).

**Page 4 & 5: Our Call Center (Generic Issue) Performance Dashboard** (Refer to `ChatBot Journey Insights and Performance Dashboard.pdf` - Pages 4 & 5)

* **Purpose:** Provides a detailed analysis of the performance of the primary "Our Call Center," covering call handling, resolution, and agent-specific metrics.

* **Filters:** Date, District, Unique vs. Total Count, Scheme, Issue Resolved, Constituency (Page 5).

* **Key Visuals & Insights:**

  * **Total Chat's transferred (Card):** Volume of chats transferred to this center.

  * **Total Chat Call's done (Card):** Volume of chats handled.

  * **Call done by Our Call Center (Card):** Total calls handled by this center.

  * **Total Missed Call's transferred to Call Center (Card):** Missed calls transferred here.

  * **Total Missed Call done by Call Center (Card):** Missed calls handled by this center.

  * **Call Resolution Status by Our Call Center (Bar Chart):** Detailed breakdown of follow-up required, resolution provided, and escalation needed.

  * **Top Districts by Total Call Done (Bar Chart):** Ranks districts by call volume handled by this center.

  * **Total Call count by Scheme Wise (Bar Chart):** Breakdown of call volume by Yojana scheme.

  * **Top Issues Scheme Wise (Bar Chart):** Identifies most frequent issues handled by scheme.

  * **Total Call count by Gender/Age Bucket (Bar Charts):** Demographic insights for calls handled by this center.

  * **Total Call count by Dialer Status (Bar Chart):** Shows status of calls (ANSWERED, ABANDONED, MISSED, NOT-ANSWERED).

  * **Total Call count by Call Type (Bar Chart):** Breaks down calls by type (Enquiry, Complaint, Unanswered, Short).

  * **Top Callers by Count (Bar Chart):** Identifies high-volume callers.

  * **Top Question by Count (Bar Chart):** Lists most frequently asked questions.

**Page 6 & 7: District Office Call Center Dashboard**

* **Purpose:** Dedicated analysis for the District Office Call Center, similar to the "Our Call Center" but focusing on its specific operations and citizen interactions.

* **Filters:** Date, Gender, Unique vs. Total Count, Issue Resolved, District, Scheme.

* **Key Visuals & Insights:**

  * **Total Calls Transferred/Done (Cards):** Overall volume metrics for this center.

  * **Gender/Language Wise Total Calls (Bar Charts):** Demographic and language insights for interactions.

  * **Dialer Status Wise Total Calls (Bar Chart):** Breakdown of call statuses.

  * **Scheme Assistance Wise Total Calls (Bar Chart):** Insights into Yojana schemes handled.

  * **Age Group Wise Total Calls (Bar Chart):** Age distribution of citizens handled.

  * **Scheme Wise Total Calls (Bar Chart):** Details on schemes.

  * **Dialer Comments Wise Total Calls (Bar Chart):** Identifies common comments/outcomes (e.g., "Call escalate," "Call back no response").

  * **Top Districts by Call Count (Bar Chart):** Geographic insights for this center.

  * **Call Type Wise Total Calls (Bar Chart):** Breakdown by call type.

  * **Caller Wise Total Calls Count (Bar Chart):** Top callers.

  * **Question wise Total Count (Bar Chart):** Most common questions.

**Page 8 & 9: National Councillor Office Call Center Dashboard**

* **Purpose:** Dedicated analysis for the National Councillor Office Call Center, providing insights into its specific operational metrics and citizen interactions.

* **Filters:** Date, Gender, Unique vs. Total Count, Issue Resolved, District, Scheme.

* **Key Visuals & Insights:**

  * **Total Calls Transferred/Done (Cards):** Overall volume metrics for this center.

  * **Gender/Language Wise Total Calls (Bar Charts):** Demographic and language insights for interactions.

  * **Dialer Status Wise Total Calls (Bar Chart):** Breakdown of call statuses.

  * **Scheme Assistance Wise Total Calls (Bar Chart):** Insights into Yojana schemes handled.

  * **Age Group Wise Total Calls (Bar Chart):** Age distribution of citizens handled.

  * **Scheme Wise Total Calls (Bar Chart):** Details on schemes.

  * **Dialer Comments Wise Total Calls (Bar Chart):** Identifies common comments/outcomes (e.g., "Call escalate," "Call back 2 PM," "Issue already solved").

  * **Top Districts by Call Count (Bar Chart):** Geographic insights for this center.

  * **Call Type Wise Total Calls (Bar Chart):** Breakdown by call type.

  * **Caller Wise Total Calls Count (Bar Chart):** Top callers.

  * **Question wise Total Count (Bar Chart):** Most common questions (e.g., "Can someone help me complete the application...").

This page-by-page breakdown, combined with the DAX explanations, provides a comprehensive understanding of the dashboard's functionality and the insights it delivers.

## ⚙️ Tools and Technologies Used

* **Data Cleaning & Preprocessing:** Python (Pandas, \`deep_translator\`, \`ThreadPoolExecutor\`)

* **Data Modeling & Analysis:** Microsoft Power BI (including advanced DAX for custom measures)

* **Data Visualization:** Microsoft Power BI

* **Data Storage:** Excel/CSV files

## 🚀 How to Use / Run the Project

This project is delivered as a Power BI dashboard. To explore the insights:

1.  **Download Power BI Desktop:** Ensure you have Microsoft Power BI Desktop installed on your machine.
2.  **Open the .pbix file:** Open the provided Power BI project file with Power BI Desktop.
3.  **Interact with the Dashboard:** You can use the various filters (Date, District, Unique vs. Total Count, Scheme, Issue Resolved) and interact with charts to drill down into specific data segments and gain further insights.

## 💼 Business Impact / Why this project is valuable

This Chatbot Journey Insights and Performance Dashboard delivers significant business value by:

* **✅ Optimizing Service Delivery:** Provides a unified view of the citizen journey, helping identify bottlenecks and areas for improvement in multi-channel support operations.

* **📈 Enhancing Operational Efficiency:** Insights into call/chat volumes, resolution rates, and agent performance enable better resource allocation and workforce management.

* **🎯 Improving Citizen Satisfaction:** By understanding common issues and resolution statuses, organizations can proactively address pain points and enhance the overall citizen experience.

* **💡 Supporting Strategic Decisions:** The dashboard empowers management with data-driven insights to refine communication strategies, allocate resources effectively across different call centers, and prioritize scheme-related support.

* **📊 Enabling Data-Driven Performance Management:** Facilitates continuous monitoring and evaluation of support channels, leading to a more efficient and effective citizen interaction ecosystem.

## Contact Information / About Me 📧

**Devanapalli Ruthwik**
**Senior Data Analyst | Business Consultant**

**Email**: ruthwik.devanapalli@gmail.com
**LinkedIn**: <https://www.linkedin.com/in/ruthwik-devanapalli/>

This project is intended for educational, portfolio, and demonstration purposes. Attribution appreciated when used or adapted.
------------------------------------------------------------------------------------------------------------------------------------------------