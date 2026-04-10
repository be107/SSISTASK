# Telecom DWH - SSIS ETL Pipeline

## 📌 Overview
An end-to-end ETL pipeline built with **SQL Server Integration Services (SSIS)** to process telecom transaction CSV files, apply transformation and validation rules, and load data into a SQL Server Data Warehouse.

---

## 🔄 Pipeline Flow
CSV Files → Derived Column → Lookup → Union All → Data Conversion → Conditional Split
↓              ↓
fact_transaction   rejected_records

---

## ⚙️ Features

- **Foreach Loop Container** — automatically processes all CSV files in a folder
- **Lookup Transformation** — joins IMSI with reference table to get `subscriber_id`; defaults to `-99999` if not found
- **Derived Column** — extracts `TAC` (first 8 chars) and `SNR` (last 6 chars) from IMEI
- **Conditional Split** — routes invalid/null records to a separate rejected table
- **File Name Tracking** — records the source CSV file name with each rejected record
- **Archive Task** — moves processed files to an archive folder after successful load

---

## 🗃️ Database

- **Target Table:** `SSIS_Telecom_DB.dbo.fact_transaction`
- **Rejected Table:** `SSIS_Telecom_DB.dbo.rejected_records`

---

## 📋 Mapping Rules

| Source Column | Rule | Target Column |
|--------------|------|---------------|
| ID | As-is | transaction_id |
| IMSI | Reject if null | imsi |
| IMSI | Lookup → subscriber_id; -99999 if not found | subscriber_id |
| IMEI | First 8 chars; -99999 if null or length < 14 | tac |
| IMEI | Last 6 chars; -99999 if null or length < 14 | snr |
| IMEI | As-is | imei |
| CELL | Reject if null | cell |
| LAC | Reject if null | lac |
| EVENT_TYPE | As-is | event_type |
| EVENT_TS | Reject if null or invalid | event_ts |

---

## 🚀 How to Run

1. Clone the repository
2. Open `Package.dtsx` in **Visual Studio** with SSIS installed
3. Update connection strings in **Connection Managers**
4. Set source folder path in **Foreach Loop Container**
5. Run the package

---

## 🛠️ Requirements

- SQL Server 2019+
- Visual Studio 2019+ with SSIS extension
- SSDT (SQL Server Data Tools)

---

## 📁 Project Structure

├── Package.dtsx          # Main SSIS package
├── README.md             # Project documentation
└── Materials/
├── Source Files/     # Input CSV files
└── Archive/          # Processed files
---

## 👤 Author
Habiba Hussam Mohamed
