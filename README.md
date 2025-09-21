# 🌍 Carbon Emissions Automation

[![FastAPI](https://img.shields.io/badge/API-FastAPI-brightgreen.svg)](https://yassine123z-emissionfactor-mapper2.hf.space/docs#/)
[![Hugging Face Spaces](https://img.shields.io/badge/HF-Spaces-orange.svg)](https://yassine123z-emissionfactor-mapper2.hf.space/)


> End-to-end pipeline that converts raw transaction and invoice data into activity-based CO₂e estimates.  
> Combines emission factor consolidation, NLP-powered matching, a FastAPI microservice, and Power BI dashboards for exploration & reporting.

---

## 📌 Table of Contents

- [Project Overview](#project-overview)
- [Why this project matters](#why-this-project-matters)
- [Tech Stack](#tech-stack) 
- [How the automation logic works)](#How-the-automation-logic-works)
- [Demo / Screenshots](#demo--screenshots)
- [Emission factors data sources](#Emission-factors-data-sources)
- [Repository structure](#repository-structure)
- [Contact](#contact)
- [License](#license)

---

## 🧭 Project Overview

This prototype demonstrates a production-like flow for converting invoice/transaction text into estimated greenhouse gas emissions:

1. Collect and unify emission factors from multiple sources (ADEME, EXIOBASE, Climatiq).
2. Standardize and enrich the factor table (GHG protocol categories, ISO mapping, units).
3. Clean and normalize client transaction data with Power Query / Fabric.
4. Match transactions to emission factors using embeddings + a small re-ranker (Hugging Face models).
5. Expose matching as a FastAPI endpoint for integration.
6. Visualize transaction-level and aggregated emissions in Power BI.


---

## 🔥 Why this project matters

- Most Scope 3 emissions are hidden in invoices and supplier data — manual mapping is slow and inconsistent.  
- This pipeline reduces manual effort and increases repeatability and explainability by combining deterministic rules with semantic matching.  
- The prototype shows a pragmatic path from raw data to decision-ready KPIs and dashboards.

---

## 🛠️ Tech stack

- **Language:** Python  
- **API:** FastAPI + Uvicorn  
- **NLP & Embeddings:** Hugging Face Transformers, SentenceTransformers, sentence-transformers (`all-MiniLM` or similar)  
- **Data processing:** pandas, Power Query (M) for Microsoft Fabric 
- **Dashboard:** Power BI

---
## ⚙️ How the automation logic works

The pipeline automates CO₂e estimation in five layers, turning messy invoice/transaction data into decision-ready dashboards:


1. **Client Transaction Data**  
   - Collect raw client data (invoices, ERP exports, procurement spreadsheets).  
   - Clean and normalize fields (units, currencies, suppliers).  
   - Output: structured transaction table in a standard format.  

2. **Emission Factor Data Collection**  
   - Gather emission factors from multiple sources (ADEME, EXIOBASE, DEFRA, Climatiq, etc.).  
   - Use APIs + Power Query Dataflows to keep data updated.  
   - Clean, standardize, and unify these into a single emission factor database.  

3. **Category Structure Table**  
   - Define a structured table of categories (Category 1, Category 2, etc.) at the level of detail required.  
   - Map these categories to **GHG Protocol categories**, **ISO codes**, and **emission scopes (1, 2, 3)**.  

<img src="5.%20Images/table_structure.png" alt="Power BI Dashboard" width="600"/>

4. **Emission Factor Alignment**  
   - Link the structured category table to the emission factor database.  
   - Result: a harmonized emission factor table aligned with standards and ready for matching.  

  ![Power BI Dashboard](5.%20Images/EmissionFactor_Structure.png)  

5. **AI & NLP Matching**  
   - Map client transactions to emission factors using embeddings (Sentence Transformers).  
   - Match based on semantic similarity between client descriptions and factor categories.  

  ![Power BI Dashboard](5.%20Images/NLP_mapping.png)  

6. **Human-in-the-Loop Review**  
   - Use a dedicated Power BI dashboard to review low-confidence matches.  
   - Quickly correct and approve mappings for auditability.  

7. **Emissions Overview**  
   - Calculate emissions with the formula:  
     ```text
     Emissions (CO₂e) = Activity Data (quantity) × Emission Factor
     ```  
   - Apply unit conversions automatically (e.g., liters ↔ MJ ↔ kgCO₂e).  
   - Visualize results in dashboards: KPIs, supplier/category trends, audit trail, and full traceability to the original invoice line.  

---

## 📷 Demo / Screenshots  

The prototype includes dashboards that illustrate the full workflow — from factor preparation to AI-assisted mapping and final emissions insights:  

- **Emission Factor Management**  
  ![Power BI Dashboard](5.%20Dashboards/EMFA.png)  
  *View and validate emission factors before and after cleaning/standardization. Ensures unit consistency, metadata alignment, and quality across sources.*  

- **Matching Review Dashboard**  
  ![Power BI Dashboard](5.%20Dashboards/Review-Match-Dashboard.png)  
  *AI models accelerate transaction-to-factor mapping, but human review remains essential. This dashboard highlights low-confidence matches and makes corrections fast and transparent.*  

- **Emissions Overview**  
  ![Power BI Dashboard](5.%20Dashboards/Emissions-Overview-Dashboard.png)  
  *Aggregate all processed transactions into clear KPIs and trends. Drill down by supplier, category, or time period, with full traceability back to each invoice line.*  


---

## 🚀 Live Demo

Your API is already deployed here:  
👉 [Emission Factor Mapper API (FastAPI on Hugging Face Spaces)](https://yassine123z-emissionfactor-mapper2.hf.space/docs#/)

---

## 🌐 Emission factors data sources  

The pipeline consolidates emission factors from **multiple international and open databases** to ensure coverage and comparability:  

- [**ADEME (Base Carbone, France)**](https://data.ademe.fr/datasets/base-carbone) → widely used in Europe for corporate carbon reporting.  
- [**EXIOBASE**](https://www.exiobase.eu/) → multi-regional input–output database, useful for Scope 3 and supply chain analysis.  
- [**DEFRA (UK Government Factors)**](https://www.gov.uk/government/collections/government-conversion-factors-for-company-reporting) → official factors for energy, transport, and procurement categories.  
- [**Climatiq API**](https://www.climatiq.io/) → programmatic access to curated emission factors from multiple sources.  
- **Other optional sources:** [Ecoinvent](https://ecoinvent.org/), [EPA (US)](https://www.epa.gov/climateleadership/ghg-emission-factors-hub), or company-specific databases (can be integrated into the workflow).  

All factors are **cleaned, standardized, and aligned** to:  
- **GHG Protocol categories**  
- **ISO standards**  
- **Scopes 1, 2, and 3**  

This ensures interoperability across reporting frameworks and compatibility with audit requirements.  


---

## 📂 Repository structure

This repository contains the FastAPI app along with the supporting resources for data ingestion (ETL), AI-based mapping, and dashboards.

1. Data → Raw and processed input datasets

2. Emission Factor Data flows/1ADEME → Reference datasets for emission factors (e.g., ADEME)

3. AI Mapping → Scripts and models for automated mapping & semantic matching

4. FastAPI → API backend to serve data and models

5. Dashboards → Visualization layer (Power BI, Streamlit, or other BI tools)

   README.md → Documentation and usage guide


---

<!-- CONTACT -->
## Contact

Yassine Zamit - [linkedin-url]: https://linkedin.com/in/yassine-zamit/ - yassine.zamit@etudiant-enit.utm.tn

Project Link: [https://github.com/yassineeea/Carbon-Emissions-Automation](https://github.com/yassineeea/Carbon-Emissions-Automation)




