# Algorithmic Complexity Analysis of Ensemble Methods for Real-Time Disease Outbreak Prediction

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

## Overview

This repository contains the code, data, and analysis for a research project investigating the theoretical and empirical computational complexity of ensemble machine learning methods for real-time malaria outbreak prediction in resource-limited settings.

**Research Duration:** 13 weeks 

**Primary Focus:** Comparing Random Forest and LightGBM on standard CPU hardware for malaria surveillance in Uganda.

---

## Research Questions

### Primary Research Question
*What are the theoretical and empirical time/space complexity bounds of Random Forest and LightGBM for real-time malaria outbreak prediction, and which configurations meet resource-limited deployment constraints on standard CPU hardware?*

### Secondary Research Questions
1. What are the training and inference time complexity bounds for Random Forest and LightGBM as functions of dataset size (n), feature dimensionality (d), number of trees (k), and tree depth (h)?
2. How closely do empirical runtime measurements align with theoretical complexity predictions for malaria outbreak prediction tasks using Uganda surveillance data?
3. Which ensemble method configurations provide the optimal trade-off between prediction accuracy (target AUC ≥ 0.85) and computational cost for real-time malaria outbreak prediction on standard CPU hardware?

---

## Data Source

**Primary:** Uganda National Health Observatory - Malaria Surveillance Data  
**URL:** https://nho.health.go.ug/themes/Malaria

**Raw data files:**
- `Export_Malaria_Admissions_(Reported).csv` - Source: Uganda Health Data Observatory, Ministry of Health Uganda. https://nho.health.go.ug
- `health_data_3_13_2026.csv` - Source: Uganda Health Data Observatory, Ministry of Health Uganda. https://nho.health.go.ug

**Features:**
- Time-series malaria case data
- District/regional geographic granularity
- Testing rates and positivity rates
- Publicly accessible aggregated data

---

## Methods

### Ensemble Methods
- **Random Forest** - Baseline ensemble with well-studied complexity
- **LightGBM** - CPU-optimized gradient boosting (histogram-based, leaf-wise growth)

### Baseline Comparisons
- Logistic Regression
- Decision Tree

### Computational Constraints
- **Hardware:** Standard CPU (Intel i5/i7 or AMD Ryzen 5/7, 8-16GB RAM)
- **Prediction Latency:** < 1 second
- **Training Time:** < 6 hours (for daily model updates)

---

## Repository Structure

```
ensemble-outbreak-complexity/
├── README.md                  # This file
├── pyproject.toml             # Python dependencies and project config
├── .gitignore                # Files to exclude from version control
│
├── data/                     # Data files (excluded from git)
│   ├── raw/                  # Original data from Uganda Health Observatory
│   ├── processed/            # Cleaned and preprocessed data
│   └── README.md             # Data documentation
│
├── notebooks/                # Jupyter notebooks for exploration and analysis
│   ├── 01_data_exploration.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_complexity_analysis.ipynb
│   └── 04_results_visualization.ipynb
│
├── src/                      # Source code
│   ├── __init__.py
│   ├── data_processing.py    # Data loading and cleaning
│   ├── models.py             # Model implementations
│   ├── complexity.py         # Complexity measurement utilities
│   ├── evaluation.py         # Model evaluation metrics
│   └── utils.py              # Helper functions
│
├── experiments/              # Experiment scripts and configs
│   ├── configs/              # Hyperparameter configurations
│   ├── run_experiments.py    # Main experiment runner
│   └── profiling.py          # Performance profiling scripts
│
├── results/                  # Experimental results
│   ├── figures/              # Plots and visualizations
│   ├── tables/               # Result tables
│   └── logs/                 # Training logs and timing data
│
├── docs/                     # Documentation and reports
│   ├── scope_definition.md   # Research scope document
│   ├── literature_review.md  # Key papers and notes
│   ├── complexity_derivations.md  # Theoretical analysis
│   └── weekly_progress/      # Weekly progress reports
│
└── paper/                    # Manuscript drafts
    ├── main.tex              # LaTeX main file
    ├── figures/              # Paper figures
    └── references.bib        # Bibliography
```

---

## Getting Started

### Prerequisites
- Python 3.8 or higher
- [uv](https://docs.astral.sh/uv/) package manager
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/ensemble-outbreak-complexity.git
   cd ensemble-outbreak-complexity
   ```

2. **Install dependencies**
   ```bash
   uv sync
   ```

### Quick Start

1. **Download malaria data**
   ```bash
   uv run python src/data_processing.py --download
   ```

2. **Run exploratory data analysis**
   ```bash
   uv run jupyter notebook notebooks/01_data_exploration.ipynb
   ```

3. **Train baseline models**
   ```bash
   uv run python experiments/run_experiments.py --config experiments/configs/baseline.yaml
   ```

---

## Dependencies

Core libraries:
- `scikit-learn` - Random Forest implementation
- `lightgbm` - LightGBM implementation
- `pandas` - Data manipulation
- `numpy` - Numerical computing
- `matplotlib` / `seaborn` - Visualization
- `jupyter` - Interactive notebooks
- `memory_profiler` - Memory profiling
- `line_profiler` - Line-by-line profiling

See `pyproject.toml` for complete list with versions.

---

## 📈 Current Progress

**Week 3 Status:**
- [x] Research scope defined
- [x] Literature review in progress
- [x] Data source identified
- [ ] Data downloaded and preprocessed
- [ ] Complexity derivations complete
- [ ] Baseline models implemented
- [ ] Experiments running
- [ ] Results analysis
- [ ] Paper draft

See `docs/weekly_progress/` for detailed updates.

---

## Key Results

*(To be updated as experiments complete)*

### Preliminary Findings
- Theoretical complexity bounds: TBD
- Empirical validation: TBD
- Optimal configurations: TBD

---

## References

Key papers for this research:

1. **Louppe, G. (2014).** *Understanding Random Forests: From Theory to Practice.* PhD Thesis, University of Liège. [arXiv:1407.7502](https://arxiv.org/abs/1407.7502)

2. **Ke, G., Meng, Q., Finley, T., et al. (2017).** *LightGBM: A Highly Efficient Gradient Boosting Decision Tree.* NIPS 2017. [Link](https://papers.nips.cc/paper/2017/hash/6449f44a102fde848669bdd9eb6b76fa-Abstract.html)

3. **Breiman, L. (2001).** *Random Forests.* Machine Learning, 45(1), 5-32.

4. **Chen, T., & Guestrin, C. (2016).** *XGBoost: A Scalable Tree Boosting System.* KDD 2016.

See `paper/references.bib` for complete bibliography.

---

## Contributing

This is a research project for academic purposes. If you find issues or have suggestions, please open an issue or contact the author.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Author

**Ayiko Andrew**
- Institution: Muni University
- Email: 2501202252@muni.ac.ug
- Advisor: None

---

## Acknowledgments

- Uganda Ministry of Health for providing open access to malaria surveillance data via the Uganda National Health Observatory
- Muni University for research support
- Course instructors and peers for feedback and guidance

---

## Citation

If you use this code or reference this research, please cite:

```bibtex
@misc{ayikoandrew2026malaria,
  title={Algorithmic Complexity Analysis of Ensemble Methods for Real-Time Malaria Outbreak Prediction},
  author={Ayiko Andrew},
  year={2026},
  note={Research project, Muni University}
}
```

---


Last Updated: March 13, 2026
