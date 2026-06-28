# IFRS9 Expected Credit Loss Model
### Python | scikit-learn | pandas | matplotlib
**Vishakha Raj** | Incoming MSc Financial Risk Management, 
Trinity College Dublin | Ex-KPMG Singapore Financial Services Audit

---

## Project Summary
Built a complete IFRS9-compliant Expected Credit Loss (ECL) model on 
78,251 Lending Club loans, estimating the three components of credit loss 
(PD, LGD, EAD) and applying EBA-calibrated recession stress testing.

Motivated by direct experience reviewing Stage 2/3 borrower classifications 
and IFRS9 ECL models during financial services audit at KPMG Singapore — 
this project flips that perspective from reviewer to builder.

---

## Key Results

| Metric | Base Case | EBA Stress Scenario |
|--------|-----------|-------------------|
| Portfolio EAD | $1.15 billion | $1.15 billion |
| Total ECL | $202 million | $337 million |
| ECL Rate | 17.59% | 29.30% |
| ECL Increase | — | +66.6% (+$134M) |
| Stage 2→3 Migration | — | 4,258 loans (70.2%) |
| PD Model AUC-ROC | 0.725 | — |

---

## Methodology

### 1. Data
- Source: Lending Club public loan dataset (Kaggle, 2007–2018)
- Sample: 78,251 completed loans (Fully Paid or Charged Off)
- Overall default rate: 20.4%

### 2. PD Model — Logistic Regression
- 8 features: FICO score, DTI, annual income, loan amount, 
  term, interest rate, employment length, recent inquiries
- Train/test split: 80/20 with stratification
- AUC-ROC: 0.725 (strong discriminatory power for retail credit)
- Key finding: Interest rate and loan term are strongest 
  default predictors; FICO and income are strongest 
  protective factors

### 3. LGD Estimation
- Observed LGD on defaulted loans: 92.0%
- Model assumption: 45% Basel III supervisory floor (base)
- Rationale: Regulatory floor provides conservative, 
  auditable benchmark; observed 92% reflects P2P-specific 
  recovery patterns not representative of bank portfolios

### 4. IFRS9 Staging (EBA/GL/2017/06)
- Stage 1: PD < 5% → 12-month ECL
- Stage 2: PD 5–20% → Lifetime ECL (SICR triggered)
- Stage 3: PD > 20% → Lifetime ECL (credit-impaired)
- Result: 0% Stage 1, 7.8% Stage 2, 92.2% Stage 3

### 5. Stress Testing
- Scenario: EBA 2023 adverse (GDP -3.3% over 3 years)
- PD multiplier: ×1.40 | LGD: 45% → 55%
- ECL impact: +66.6% | Additional provisions: $134.5M
- Stage migration: 70.2% of Stage 2 book → Stage 3

---

## Irish Banking Context
Results are benchmarked against Irish regulatory frameworks:
- IFRS9 staging thresholds aligned to EBA/GL/2017/06
- Stress parameters calibrated to EBA 2023 adverse scenario
- LGD assumptions reference Basel III CRR supervisory floors
- Stage migration analysis reflects CBI guidance on SICR 
  triggers issued during COVID-19 portfolio review cycle

For comparison: AIB and Bank of Ireland report approximately 
85% of retail mortgage portfolios in Stage 1, reflecting 
secured lending and CBI macro-prudential LTV restrictions — 
contrasting with the 0% Stage 1 observed in this P2P dataset.

---

## Tools & Libraries
Python 3.14 | pandas | numpy | scikit-learn | 
matplotlib | seaborn

---

## Files
- `credit_risk_model.ipynb` — full annotated notebook
- `eda_charts.png` — exploratory data analysis
- `pd_model_charts.png` — ROC curve and PD distribution
- `ecl_final_summary.png` — ECL summary and stress test
