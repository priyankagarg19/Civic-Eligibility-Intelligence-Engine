# 🚀 Civic Eligibility Intelligence Engine (CEIE)

> AI-powered system that matches citizens to government welfare schemes automatically and detects missed benefits.

---

## 🧩 Problem

India has hundreds of welfare schemes, but:

* Eligibility rules are buried inside complex PDFs
* Criteria vary across schemes and states
* Citizens cannot manually compare requirements
* Rural communities rely on middlemen
* Eligible people miss benefits

This leads to underutilization of public funds and welfare leakage.

---

## 💡 Proposed Solution

CEIE converts unstructured scheme documents into structured eligibility intelligence using AI.

The system:

1. Extracts eligibility rules from scheme PDFs
2. Captures structured citizen profiles
3. Matches profiles against scheme criteria
4. Calculates eligibility scores (0–100)
5. Ranks schemes by relevance
6. Generates personalized document checklists
7. Provides explainable results

---

## 🤖 Why AI is Necessary

This is not a static rule system.

AI is required because:

* Scheme rules are written in natural language
* Documents are unstructured
* Eligibility depends on multiple dynamic conditions
* Rules differ across departments and states

We use NLP to:

* Extract criteria (age, income, occupation, land size, etc.)
* Classify rule types
* Identify thresholds and operators
* Assign confidence scores
* Enable explainable eligibility decisions

---

## 🏗️ High-Level Architecture

Citizen Profile →
NLP Rule Extraction →
Eligibility Matching Engine →
Scoring & Ranking →
Document Checklist Generator →
Explainable Output

---

## 🧠 Core Modules

* **Profile Intake Module** – Structured citizen data capture
* **NLP Extraction Engine** – Converts scheme text into rules
* **Eligibility Matching Engine** – Compares profile vs criteria
* **Scoring Engine** – Weighted eligibility scoring
* **Ranking System** – Prioritizes high-impact schemes
* **Explainability Engine** – Transparent reasoning for eligibility

---

## ⚙️ Tech Stack (Proposed MVP)

* Python + FastAPI
* spaCy (NLP)
* PostgreSQL
* Redis (caching)
* React (Frontend)
* Docker (Deployment)

---

## 🎯 Target Beneficiaries

* Rural households
* Small farmers
* Informal workers
* NGOs
* Government outreach teams

---

## 📈 Expected Impact

* Increased scheme enrollment
* Reduced information asymmetry
* Lower dependency on middlemen
* Better utilization of public welfare budgets
* Scalable across districts and states

---
