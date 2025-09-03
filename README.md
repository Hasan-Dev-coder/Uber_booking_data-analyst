# 🚖 Uber Booking Data Analysis

This project analyses **Uber-style ride bookings** for the NCR (Noida, Delhi, Ghaziabad, Gurgaon) city region using **SQL, Google Sheets, and Power BI**.  
Goal: clean, transform, and visualise booking data to extract actionable business insights (demand trends, cancellations, payment mix, peak hours, ride completions, driver & customer ratings).

---

## 📂 Project Structure
- **Dataset**: `ncr_ride_bookings.csv` (Raw data file used for the analysis)  
- **SQL Scripts**: `queries.sql` (Data cleaning, transformation, KPI calculations)  
- **Google Sheets**: `sheets/` (Pivot tables and ad-hoc summaries)  
- **Power BI Dashboard**: `Uber.pbix` – Interactive dashboard for business insights  

---

## 🔹 Dataset Source (Important)
**Dataset provenance:** This dataset is **synthetic** and was generated using ChatGPT for the purpose of this analysis and demonstration. It was created to mimic realistic ride-booking data for NCR(Noida, Delhi, Ghaziabad, Gurgaon) with **1.5 lac rows** covering one month.

**Generation Prompt (used with ChatGPT):**

> Please create a spreadsheet with 150000 lac rows for NCR. Give the following columns.  
> The data will be for 1 month. Use the following column -  
> 1. Date  
> 2. Time  
> 3. Booking ID  
> 4. Booking Status  
> 5. Customer ID  
> 6. Vehicle Type  
> - Auto  
> - Prime Plus  
> - Prime Sedan  
> - Mini  
> - Bike  
> - eBike  
> - Prime SUV  
> 7. Pickup Location (Create dummy location points. Take any 50 areas from Bangalore)  
> 8. Drop Location (Take from dummy pickup locations)  
> 9. Avg VTAT (Time taken to arrive at the vehicle)  
> 10. Avg CTAT (Time taken to arrive at the Customer)  
> 11. Cancelled Rides by Customer  
> 12. Reason for cancelling by Customer  
> - Driver is not moving towards the pickup location  
> - Driver asked to cancel  
> - AC is not working (Only for 4-wheelers)  
> - Change of plans  
> - Wrong Address  
> 13. Cancelled Rides by Driver  
> - Personal & Car related issues  
> - Customer-related issue  
> - The customer was coughing/sick  
> - More than the permitted people in there  
> 14. Incomplete Rides  
> 15. Incomplete Rides Reason  
> - Customer Demand  
> - Vehicle Breakdown  
> - Other Issue  
> 16. Booking Value  
> 17. Ride Distance  
> 18. Driver Ratings  
> 19. Customer Rating  
> Keep the overall booking status success for this data at 62%. If the booking status is successful, then only fare charge ratings, average VTAT, average CTAT, and other data will be there.

**Why synthetic?**  
- Using a synthetic dataset ensures privacy (no PII from real users) and lets the project demonstrate realistic data engineering and analytics workflows.  
- The dataset structure and distributions were intentionally designed to mirror real-world patterns (success vs cancelled rate, peak hours, rating distributions, etc.) so analyses and dashboards behave like production data.

> **Note for reviewers/recruiters:** If desired, the dataset can be replaced with a real or Kaggle dataset. This repo demonstrates the full pipeline (SQL → Google Sheets → Power BI) and is reproducible using the provided prompt.

---

## 🔹 Steps Performed

### 1. Data Generation & Ingestion
- Synthetic data (100,000 rows) imported into SQL (MySQL / PostgreSQL).  
- Raw CSV also loaded into Google Sheets for quick checks and pivot summaries.

### 2. Data Cleaning (SQL & Google Sheets)
- Removed duplicates and standardized datetime formats.  
- Filled or flagged missing values for fare/distance when booking was not successful.  
- Calculated derived columns: `Trip_Duration_minutes`, `Revenue_after_discount`, `Is_Successful` (boolean), `VTAT_seconds`, `CTAT_seconds`.  
- Ensured overall success rate ~**62%** as requested.

### 3. Data Transformation (Star schema suggestion)
- Created a simple star schema for analytics:
  - `fact_rides` (booking_id, date_id, time_id, customer_id, vehicle_type_id, pickup_id, drop_id, status, fare, distance, duration, vt_at, ct_at, driver_rating, customer_rating, ...)
  - dimension tables: `dim_date`, `dim_time`, `dim_vehicle_type`, `dim_location`, `dim_customer` (sampled)

### 4. Analysis (SQL queries)
- KPIs computed: Total rides, Completed rides, Revenue, Average fare, Avg trip distance, Avg VTAT/CTAT, Cancellation rate, Top pickup/drop locations.  
- Time-series and peak-hour analysis to identify demand windows.  
- Cancellation reasons breakdown (customer vs driver) and incomplete ride analysis.

### 5. Reporting (Power BI)
- Interactive dashboard includes:
  - **Top KPIs**: Total Rides, Completed Rides, Revenue, Avg Fare, Cancellation %  
  - **Temporal Analysis**: Hourly heatmap, day-of-week patterns  
  - **Geographic**: Top pickup and drop locations (mapped visuals or bar charts)  
  - **Operational**: VTAT/CTAT distributions, Driver & Customer Rating trends  
  - **Cancellation Insights**: Most frequent reasons and correlation with time/area

---

## 📈 Key Insights (sample)
- Overall completion rate was set to **~62%** in the synthetic data.  
- Peak demand observed in evening hours (6–9 PM).  
- Cash and UPI were the most common payment methods (if included).  
- Top 5 pickup areas contributed ~40% of demand (illustrative).

---

## 🛠 Tools & Technologies
- **SQL** (MySQL / PostgreSQL) – Data cleaning, aggregation, and analysis  
- **Google Sheets** – Quick pivot tables and validation checks  
- **Power BI Desktop** – Dashboard development (`Uber.pbix`)  

---

## 📜 How to reproduce
1. Clone the repository:  
   ```bash
   git clone https://github.com/Haan271998/Uber-Booking-Analysis.git
   cd Uber-Booking-Analysis
