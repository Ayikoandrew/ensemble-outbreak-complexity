# LITERATURE REVIEW

## Algorithmic Complexity Analysis of Ensemble Methods for Real-Time Disease Outbreak Prediction

**Research Project**  
**March 2026**

---

## Abstract

This literature review examines the theoretical and empirical foundations of algorithmic complexity analysis in ensemble machine learning methods, with specific focus on Random Forest and LightGBM for disease outbreak prediction. We survey key papers on ensemble method complexity, CPU-optimized implementations, and applications to epidemiological forecasting. The review identifies a critical gap: while both Random Forest and LightGBM are widely used for outbreak prediction, systematic complexity analysis comparing their performance on standard CPU hardware in resource-limited settings is limited. This gap motivates our research question: what are the practical complexity trade-offs between these methods for real-time disease outbreak prediction in Uganda?

---

## 1. Introduction

Disease outbreak prediction requires algorithms that can process surveillance data quickly enough to inform timely public health responses. In resource-limited settings such as Uganda, where GPU acceleration may not be available, the choice between ensemble methods becomes critical. This literature review examines two primary ensemble approaches—Random Forest and LightGBM—through the lens of algorithmic complexity and real-world deployment constraints.

### 1.1 Research Context

Our research focuses on Uganda malaria outbreak prediction using WHO surveillance data (140 temporal observations, 19 derived features from multiple indicators including incidence, mortality, and admissions data). The constraint of standard CPU hardware (no GPU acceleration) is not a limitation but a realistic deployment scenario for district-level health offices in Sub-Saharan Africa.

### 1.2 Review Structure

This review is organized into six thematic sections: (1) Random Forest theory and complexity, (2) Gradient Boosting and LightGBM architecture, (3) Complexity analysis methodologies, (4) Disease outbreak prediction with machine learning, (5) CPU optimization strategies, and (6) identified research gaps.

---

## 2. Random Forest: Theory and Complexity

### 2.1 Foundational Work

**Breiman (2001)** introduced Random Forest as an ensemble of decision trees trained on bootstrap samples with random feature selection. The method's popularity stems from its robustness, parallelizability, and strong empirical performance across domains.

Key properties relevant to complexity analysis: 
1. Each tree is trained independently, enabling perfect parallelization across CPU cores
2. Bootstrap sampling introduces randomness that improves generalization
3. Random feature selection at each split reduces correlation between trees while maintaining individual tree accuracy

### 2.2 Complexity Analysis

**Louppe (2014)** provides the most comprehensive theoretical analysis of Random Forest complexity in his PhD thesis *"Understanding Random Forests: From Theory to Practice."* He derives:

- **Training complexity:** *O(k · n · log(n) · d)* where k = number of trees, n = samples, d = features
- **Inference complexity:** *O(k · h)* where h = average tree depth
- **Space complexity:** *O(k · n_leaf)* for storing tree structures

The n·log(n) term arises from sorting features at each node during split selection. Louppe's analysis shows that for typical values (k=100, n=1000, d=20), Random Forest training is dominated by the sorting operations, making it sensitive to dataset size.

### 2.3 Parallelization and CPU Performance

Random Forest's parallel nature makes it well-suited for multi-core CPUs. Each tree can be trained on a separate core with no inter-process communication. Modern implementations (scikit-learn, ranger) achieve near-linear speedup with core count. For a 4-core CPU, training time approximately divides by 4.

---

## 3. Gradient Boosting and LightGBM

### 3.1 Gradient Boosting Foundations

**Friedman (2001)** introduced gradient boosting as a sequential ensemble method that builds trees to correct errors of previous iterations. Unlike Random Forest's parallel training, boosting is inherently sequential—each tree depends on the residuals from prior trees.

### 3.2 XGBoost Architecture

**Chen & Guestrin (2016)** developed XGBoost with optimizations including: second-order gradient approximation, regularization to prevent overfitting, and cache-aware access patterns. XGBoost's exact split-finding algorithm has complexity *O(n·d·k·log(n))*, similar to Random Forest but sequential rather than parallel.

### 3.3 LightGBM: CPU-Optimized Gradient Boosting

**Ke et al. (2017)** introduced LightGBM with two key innovations that reduce complexity on CPU hardware:

**1. Histogram-based split finding:** Instead of exact sorting (*O(n·log(n))* per feature), LightGBM bins continuous features into discrete histograms. Split finding then becomes *O(n + bins)*, typically *O(n)* since bins is small (default 255). This changes training complexity from *O(n·d·k·log(n))* to approximately *O(n·d·k)*.

**2. Leaf-wise tree growth:** XGBoost grows trees depth-wise (level-by-level), while LightGBM grows leaf-wise (best-leaf-first). Leaf-wise growth produces deeper, more accurate trees with fewer total splits, reducing the effective k required for comparable accuracy.

Additional CPU optimizations include Gradient-based One-Side Sampling (GOSS) which samples large-gradient instances more frequently, and Exclusive Feature Bundling (EFB) which merges mutually exclusive features. Together, these techniques make LightGBM the fastest gradient boosting implementation on standard CPUs.

### 3.4 Theoretical Complexity Comparison

The critical complexity distinction: Random Forest has *O(k·n·log(n)·d)* training but perfect parallelism, while LightGBM has *O(k·n·d)* training but sequential execution. The crossover point depends on:
1. Number of CPU cores available
2. Dataset size n
3. Number of trees k required for target accuracy

---

## 4. Complexity Analysis Methodologies

### 4.1 Empirical Profiling Techniques

Rigorous complexity analysis requires both theoretical derivation and empirical validation. Standard profiling approaches measure:
1. Wall-clock training time across varying n
2. Inference latency per sample
3. Peak memory usage during training
4. Scaling behavior (polynomial fits to validate O-notation)

### 4.2 Real-Time Constraints

For outbreak prediction systems, real-time performance requires:
1. Prediction latency < 1 second (to enable interactive decision support)
2. Training time < 6 hours (to allow daily model updates with new surveillance data)
3. Memory footprint < 4GB (deployable on district-level hardware)

---

## 5. Machine Learning for Disease Outbreak Prediction

### 5.1 Epidemiological Forecasting

**Gao et al. (2025)** review ML methods for early outbreak detection, noting that ensemble methods (Random Forest, gradient boosting) consistently outperform traditional statistical models (ARIMA, exponential smoothing) for non-linear disease dynamics.

### 5.2 Malaria-Specific Applications

Malaria outbreak prediction has unique characteristics: strong seasonal patterns, vector-borne transmission with climate dependencies, and regional heterogeneity. Ensemble methods handle these multi-factorial relationships effectively. However, existing studies focus primarily on prediction accuracy rather than computational complexity for deployment.

---

## 6. Identified Research Gaps

### 6.1 Limited Complexity Analysis in Applied Contexts

While theoretical complexity is well-established (Louppe 2014, Ke et al. 2017), empirical validation using real disease surveillance data is rare. Most papers report accuracy metrics but not training time, inference latency, or memory usage under realistic deployment constraints.

### 6.2 CPU-Only Performance Comparison

LightGBM is typically benchmarked with GPU acceleration, where its advantages are most pronounced. The CPU-only scenario—directly relevant to resource-limited settings—receives less attention. Our preliminary findings suggest a crossover point: for small ensembles (k < 20), Random Forest may be faster on CPU due to LightGBM's histogram construction overhead.

### 6.3 Small Data Regime

Most complexity analyses assume large datasets (n > 10,000). Outbreak prediction often operates with limited historical data (n = 100-500 temporal observations). In our Uganda malaria data (n=129), we observe 100% accuracy on simple models, indicating perfect separation—a regime where complexity matters more than accuracy.

---

## 7. Conclusion and Research Questions

This literature review establishes the theoretical foundation for comparing Random Forest and LightGBM complexity. The key insight: while LightGBM has superior asymptotic complexity *O(n·d·k)* versus Random Forest's *O(n·log(n)·d·k)*, the crossover point depends on ensemble size k and CPU parallelism. For small k (< 50), Random Forest's parallelizability may dominate. For large k (> 100), LightGBM's histogram optimization becomes advantageous even on CPU.

### 7.1 Guiding Research Questions

**Primary RQ:** What are the empirical complexity bounds of Random Forest versus LightGBM for malaria outbreak prediction on standard CPU hardware, and at what ensemble size (k) does LightGBM become advantageous?

**Secondary RQ1:** How closely do measured training times align with theoretical predictions *O(k·n·log(n)·d)* for Random Forest and *O(k·n·d)* for LightGBM?

**Secondary RQ2:** What configurations meet real-time constraints (< 1s inference, < 6h training) while maintaining outbreak prediction accuracy (AUC ≥ 0.85)?

---

## References

**Breiman, L. (2001).** *Random Forests.* Machine Learning, 45(1), 5-32.

**Chen, T., & Guestrin, C. (2016).** *XGBoost: A Scalable Tree Boosting System.* KDD 2016.

**Friedman, J. H. (2001).** *Greedy function approximation: A gradient boosting machine.* Annals of Statistics, 29(5), 1189-1232.

**Gao, S., et al. (2025).** *Early detection of disease outbreaks using incidence data.* PLOS Computational Biology.

**Ke, G., Meng, Q., Finley, T., et al. (2017).** *LightGBM: A Highly Efficient Gradient Boosting Decision Tree.* NIPS 2017.

**Louppe, G. (2014).** *Understanding Random Forests: From Theory to Practice.* PhD Thesis, University of Liège. arXiv:1407.7502