# 🌍 Carbon Emissions Automation

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](#license)
[![FastAPI](https://img.shields.io/badge/API-FastAPI-brightgreen.svg)](#api)
[![Hugging Face Spaces](https://img.shields.io/badge/HF-Spaces-orange.svg)](https://yassine123z-emissionfactor-mapper2.hf.space/)


> End-to-end pipeline that converts raw transaction and invoice data into activity-based CO₂e estimates.  
> Combines emission factor consolidation, NLP-powered matching, a FastAPI microservice, and Power BI dashboards for exploration & reporting.

---

## 📌 Table of Contents

- [Project Overview](#project-overview)
- [Why this project matters](#why-this-project-matters)
- [Tech Stack](#tech-stack)
- [Demo / Screenshots](#demo--screenshots)
- [Repository structure](#repository-structure)
- [Quickstart (run locally)](#quickstart-run-locally)
- [API examples](#api-examples)
- [How the matching works (high-level)](#how-the-matching-works-high-level)
- [Evaluation & Confidence](#evaluation--confidence)
- [Suggested 2–4 week pilot for Simple](#suggested-2-4-week-pilot-for-simple)
- [Roadmap & Production suggestions](#roadmap--production-suggestions)
- [Credits & data sources](#credits--data-sources)
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

4. **Emission Factor Alignment**  
   - Link the structured category table to the emission factor database.  
   - Result: a harmonized emission factor table aligned with standards and ready for matching.  

5. **AI & NLP Matching**  
   - Map client transactions to emission factors using embeddings (Sentence Transformers).  
   - Match based on semantic similarity between client descriptions and factor categories.  

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

## 📂 Repository structure

This repo contains both the **API** (FastAPI app) and supporting resources for ETL + dashboards.

