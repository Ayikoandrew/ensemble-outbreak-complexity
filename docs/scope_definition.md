# RESEARCH SCOPE DEFINITION

## UPDATED - WEEK 3

## Algorithmic Complexity Analysis of Ensemble Methods for Real-Time Malaria Outbreak Prediction

---

## Status Update (Week 3)

> **✓ Data Source Confirmed:** WHO Uganda malaria indicators (140 temporal observations, 2000-2023) plus regional admissions data (164 records, 14 regions, 2018-2024). Downloaded from Humanitarian Data Exchange (data.humdata.org).

> **✓ Baseline Experiments Complete:** Random Forest vs LightGBM profiled with k=10, h=5. Key finding: RF faster for small ensembles (267ms vs 672ms) due to parallelism advantage.

> **✓ Feature Engineering Done:** Final dataset: 129 samples, 19 features derived from malaria incidence, mortality, admissions, and spatial encodings.

> **✓ Complexity Analysis Framework:** ComplexityProfiler class implemented for systematic profiling of training time, inference latency, and memory usage.

---

## 1. Research Problem Statement

> **This research addresses the problem that existing disease outbreak prediction systems using ensemble methods lack rigorous algorithmic complexity analysis, making it unclear whether they can meet real-time processing requirements in resource-limited public health settings with standard CPU hardware, specifically for Uganda where GPU acceleration is unavailable in district health offices and rapid prediction is critical for timely intervention.**

---

## 2. Core Research Questions

### Primary Research Question

> **What are the theoretical and empirical time/space complexity bounds of Random Forest and LightGBM for real-time disease outbreak prediction, and which configurations meet resource-limited deployment constraints on standard CPU hardware?**

### Secondary Research Questions

> **SQ1:** What are the training and inference time complexity bounds for Random Forest and LightGBM as functions of dataset size (n), feature dimensionality (d), number of trees (k), and tree depth (h) for Uganda malaria data?

> **SQ2:** How closely do empirical runtime measurements align with theoretical complexity predictions *O(k·n·log(n)·d)* for Random Forest and *O(k·n·d)* for LightGBM?

> **SQ3:** At what ensemble size (k) does LightGBM's histogram-based optimization become more efficient than Random Forest's exact sorting on standard CPU hardware?

---

## 3. Disease Focus & Data Source

### Disease: Malaria (Uganda)

> **Justification:** Malaria is Uganda's leading public health burden with well-established surveillance systems providing rich historical time-series data. The seasonal outbreak patterns and continuous reporting make it ideal for algorithmic complexity analysis of real-time prediction systems.

### Primary Data Source

> **Source:** WHO Uganda Malaria Indicators via Humanitarian Data Exchange (https://data.humdata.org/dataset/who-data-for-uga)

**Data Characteristics:**

- **WHO Indicators:** 140 temporal observations (2000-2023)
- **Regional Admissions:** 164 records (14 regions, 2018-2024)
- **Final Dataset:** 129 samples, 19 features
- **Target:** Binary outbreak classification (high vs low mortality)
- **Features:** Incidence rates, mortality, admissions, ITN coverage, spatial encodings

> **Ethics Approval:** Not required (public aggregated data)

---

## 4. Methods & Models

| Method | Status | Complexity |
|--------|--------|------------|
| **Random Forest** | ✓ Implemented & Tested | *O(k·n·log(n)·d)* |
| **LightGBM** | ✓ Implemented & Tested | *O(k·n·d)* |
| Logistic Regression | ✓ Baseline | *O(n·d·i)* |
| Decision Tree | ✓ Baseline | *O(n·d·log(n))* |

---

## 5. Computational Constraints

### Hardware Specifications

- **CPU:** Standard laptop/workstation (Intel i5/i7 or AMD Ryzen, 4-8 cores)
- **RAM:** 8-16 GB
- **GPU:** None (CPU-only constraint)

### Real-Time Constraints

> **Prediction latency:** < 1 second (both methods meet this: RF 2.68ms, LGB 7.09ms)

> **Training time:** < 6 hours (both methods meet this: RF 267ms, LGB 672ms for k=10)

---

## 6. Success Criteria

- ✓ **1. Theoretical complexity bounds derived for RF and LightGBM**
- ✓ **2. Empirical profiling completed on CPU hardware**
- ⬜ **3. Crossover point identified (k where LightGBM becomes faster)**
- ✓ **4. Real-time feasibility demonstrated (both < 1s inference)**
- ✓ **5. Prediction accuracy: AUC ≥ 0.85 achieved (RF: 1.00, LGB: 0.96)**
- ⬜ **6. Comprehensive paper/thesis draft completed**

---

## 7. Week 3 Progress Summary

### ✓ Completed

- WHO malaria data acquired and processed
- Feature engineering (19 features from 3 data sources)
- Baseline complexity profiling (4 models)
- Literature review in progress
- Theoretical complexity derivations documented

### ⬜ Next Steps (Week 4-5)

- Vary ensemble size k (10, 50, 100, 200) to find crossover
- Test scalability with varying n (subsampling analysis)
- Memory profiling and visualization
- Begin writing methodology section

---

## 8. Detailed Experimental Plan

### 8.1 Baseline Experiments (✓ COMPLETE)

**Configuration:**
- k = 10 trees
- h = 5 max depth
- n = 129 samples
- d = 19 features

**Results:**

| Model | Training Time | Inference Time | Memory | Accuracy |
|-------|---------------|----------------|--------|----------|
| Random Forest | 267.56 ms | 2.68 ms | 0.061 MB | 100% |
| LightGBM | 672.39 ms | 7.09 ms | 1.095 MB | 96.2% |

**Key Finding:** RF is 2.5x faster than LightGBM for small ensembles (k=10) on CPU.

### 8.2 Hyperparameter Sweep (Week 4)

**Random Forest:**
- n_estimators: [10, 50, 100, 200, 500]
- max_depth: [5, 10, 15, 20, None]

**LightGBM:**
- num_boost_round: [10, 50, 100, 200, 500]
- num_leaves: [15, 31, 63, 127]

**Metrics:** Training time, inference latency, accuracy, memory usage

### 8.3 Scalability Analysis (Week 4-5)

**Approach:** Vary dataset size while keeping other parameters constant

**Dataset Sizes:** n = [100, 250, 500, 1000, 2000, 3000, ALL]

**Expected Results:**
- RF training time ∝ n·log(n)
- LightGBM training time ∝ n
- Crossover point: n ≈ 1000 where LightGBM becomes faster

### 8.4 Memory Profiling (Week 5)

**Measurements:**
- Peak memory during training
- Model storage size
- Inference memory footprint

**Tools:** Python `tracemalloc`, `memory_profiler`

---

## 9. Research Timeline (13 Weeks)

### Phase 1: Foundation (Weeks 1-3) ✓ COMPLETE

- ✓ Literature review
- ✓ Data acquisition and preprocessing
- ✓ Baseline model implementation
- ✓ Initial complexity analysis

### Phase 2: Experimentation (Weeks 4-7)

- Week 4: Hyperparameter sweep
- Week 5: Scalability analysis
- Week 6: Memory profiling and optimization
- Week 7: Crossover point validation

### Phase 3: Analysis (Weeks 8-10)

- Week 8: Results visualization
- Week 9: Statistical analysis and validation
- Week 10: Comparative analysis with literature

### Phase 4: Writing (Weeks 11-13)

- Week 11: Methodology and results sections
- Week 12: Discussion and conclusions
- Week 13: Final revisions and submission

---

## 10. Expected Contributions

### 10.1 Theoretical Contributions

1. **Rigorous complexity derivations** for Random Forest and LightGBM in the context of outbreak prediction
2. **Crossover point formula** relating k, n, d, and p for method selection
3. **Small data regime analysis** (n < 500) often overlooked in ML literature

### 10.2 Empirical Contributions

1. **Systematic profiling** on CPU-only hardware reflecting real deployment constraints
2. **Uganda malaria case study** with public WHO data
3. **Practical guidelines** for ensemble method selection in resource-limited settings

### 10.3 Applied Contributions

1. **Deployment recommendations** for district-level health offices
2. **Real-time feasibility validation** for outbreak prediction systems
3. **Open-source profiling framework** for complexity analysis

---

## 11. Risk Assessment and Mitigation

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Data quality issues | Low | Medium | Using WHO verified data; multiple indicators |
| Insufficient computational resources | Low | High | Cloud computing backup (Google Colab) |
| Timeline delays | Medium | Medium | 2-week buffer in 13-week schedule |
| Accuracy requirements not met | Low | High | Baseline already exceeds targets (AUC > 0.85) |
| Crossover point unclear | Medium | Medium | Extend k range to 1000 if needed |

---

## 12. Deliverables

### Academic Deliverables

1. **Research paper** (8-12 pages) for conference submission
2. **Thesis chapter** (25-40 pages) with full derivations
3. **Presentation slides** for thesis defense

### Code Deliverables

1. **GitHub repository** with all code and documentation
2. **Jupyter notebooks** for reproducibility
3. **Python package** for complexity profiling (optional)

### Data Deliverables

1. **Processed WHO malaria dataset** with documentation
2. **Experimental results** (CSV files with all measurements)
3. **Visualization suite** (PNG/PDF figures for publication)

---

## 13. Literature Foundation

### Key Papers (Must Read)

1. **Louppe (2014)** - Understanding Random Forests: From Theory to Practice
2. **Ke et al. (2017)** - LightGBM: A Highly Efficient Gradient Boosting Decision Tree
3. **Breiman (2001)** - Random Forests
4. **Chen & Guestrin (2016)** - XGBoost: A Scalable Tree Boosting System

### Supporting Papers (High Priority)

5. **Gao et al. (2025)** - Early detection of disease outbreaks using incidence data
6. **Friedman (2001)** - Greedy function approximation: A gradient boosting machine

---

## 14. Scope Boundaries

### In Scope

- ✓ Random Forest and LightGBM complexity analysis
- ✓ CPU-only hardware (no GPU acceleration)
- ✓ Malaria outbreak prediction (Uganda)
- ✓ WHO surveillance data
- ✓ Real-time constraints (< 1s inference)

### Out of Scope

- ✗ Other diseases (dengue, COVID-19)
- ✗ GPU-accelerated implementations
- ✗ Deep learning methods (LSTM, transformers)
- ✗ XGBoost detailed comparison (mention only)
- ✗ Production deployment and infrastructure

---

## 15. Ethical Considerations

### Data Privacy

- Using public aggregated data (no individual patient records)
- No personally identifiable information (PII)
- WHO data is approved for research use

### Research Integrity

- All code and data will be made publicly available
- Reproducible research practices (version control, documentation)
- Transparent reporting of limitations and negative results

### Societal Impact

- Focus on resource-limited settings (positive impact)
- No commercial interests or conflicts
- Results will be shared with Uganda Ministry of Health

---

## COMMITMENT (Week 3)

**I commit to completing this research scope by Week 13, with adjusted milestones based on Week 3 progress.**

**Student:** Ayiko Andrew  **Date:** March 19, 2026

**Advisor:**   **Date:** 

---

## Appendix A: GitHub Repository Structure

```
ensemble-outbreak-complexity/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── raw/
│   │   ├── malaria_indicators_uga.csv
│   │   └── admissions_clean.csv
│   └── processed/
│       └── malaria_features_engineered.csv
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_complexity_analysis.ipynb
│   └── 04_results_visualization.ipynb
├── src/
│   ├── models/
│   │   ├── random_forest.py
│   │   └── lightgbm_model.py
│   ├── profiling/
│   │   └── complexity_profiler.py
│   └── utils/
│       └── data_loader.py
├── experiments/
│   ├── baseline_experiment.py
│   ├── hyperparameter_sweep.py
│   └── scalability_analysis.py
├── results/
│   ├── figures/
│   └── tables/
└── docs/
    ├── literature_review.md
    ├── complexity_derivations.md
    └── methodology.md
```

---

## Appendix B: Key Metrics Tracking

### Training Time Targets

| Ensemble Size (k) | Random Forest | LightGBM | Winner |
|-------------------|---------------|----------|--------|
| k = 10 | 267.56 ms | 672.39 ms | RF ✓ |
| k = 50 | TBD | TBD | ? |
| k = 100 | TBD | TBD | ? |
| k = 200 | TBD | TBD | LGB (predicted) |
| k = 500 | TBD | TBD | LGB (predicted) |

### Inference Latency Targets

- **Target:** < 1000 ms per batch (100 samples)
- **RF (k=10):** 2.68 ms ✓ (well under budget)
- **LGB (k=10):** 7.09 ms ✓ (well under budget)

### Accuracy Targets

- **Target:** AUC ≥ 0.85, F1 ≥ 0.85
- **RF:** AUC 1.00, F1 1.00 ✓
- **LGB:** AUC 0.96, F1 0.96 ✓

Both models exceed accuracy requirements, making complexity the deciding factor.