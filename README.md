# 🌐 Global Arms Transfer Network Analysis (2000–2023)
 
Structural, centrality, and community analysis of the global arms trade, modelled as a directed weighted network of exporter → importer relationships.
 
The project is organized as a self-contained notebook that asks a single question: what does the structure of arms transfers reveal about power, dependency, and geopolitical blocs?
 
## 📓 Notebook
 
| Notebook | Focus | Key Methods |
|---|---------|-------------|
| `arms_trade_network_analysis.ipynb` | Structure, influence, and dependency in the 2000–2023 global arms transfer network | Directed graph construction (quantity-weighted, log-normalized, unweighted), degree/heavy-tail analysis, clustering, path length on the LSCC, 7 centrality measures, greedy modularity communities, composite conflict-capacity ranking, HHI import dependency |
 
The notebook is organized into ten sections:
 
| Section | Description |
|---|---------|
| **1. Setup & Data Loading** | Loads the raw transfer records, removes non-state actors (e.g. insurgent groups, alliances such as `NATO**`, `various`, `unknown`), parses order year, and imputes missing quantities with the per-category median |
| **2. Exploratory Data Analysis** | Transfers per year, top exporters/importers, weapon category breakdown, quantity distribution (raw and log), and annual trends for the top 5 exporters |
| **3. Build Network Graphs** | Aggregates transfers into three directed graphs: `G_qty` (edge weight = `SUM(quantity)`), `G_qnorm` (log-normalized quantity), and `G_uw` (unweighted) |
| **4. Degree Distribution** | In/out-degree and strength across all three graphs, plus MLE-based heavy-tail fitting (power-law vs lognormal vs exponential) with likelihood ratio tests |
| **5. Clustering Coefficients** | Average local clustering and transitivity on undirected projections, compared across weighting schemes |
| **6. Average Path Length & Diameter** | Hop-based and weighted (`-log(w / max w)`) path metrics on the largest strongly connected component |
| **7. Centrality Measures** | Degree, strength, betweenness, closeness, eigenvector, PageRank, and Katz, with a correlation matrix and top-20 ranking |
| **8. Network Visualisations** | Top 60 countries by strength drawn in four layouts (Circular, Spring, Kamada-Kawai, Spectral); node size ∝ PageRank, color = bloc, edge width ∝ volume |
| **9. Community Detection** | Greedy modularity maximization to identify geopolitical blocs, with bloc size, density, and inter-bloc flow comparisons |
| **10. Geopolitical Analysis** | Composite conflict-capacity tiers, patron-state dependency blocs, bridge countries, and a Herfindahl-Hirschman Index (HHI) of import supplier concentration |
 
---
 
## 💾 Data
 
One dataset is required, placed in the `datasets/` folder:
 
| File | Contents |
|---|-------------|
| `datasets/global_arms_transfer_2000_2023.csv` | 10,520 raw transfer records (15 columns): source, target, order/delivery date, quantity, designation, description, weapon category, and source/target coordinates |
 
After cleaning, 10,230 records remain, forming a network of **185 countries** and **1,914 directed edges** (density ≈ 0.056).
 
Edge weights use `SUM(quantity)` rather than transaction count, so a single large transfer is not treated the same as a single small one. A log-normalized variant (`log1p(quantity)`) is kept for comparison.
 
---
 
## 🔍 Key Findings
 
### Structure
 
- The network is **topologically compact but economically distant**: unweighted average path length ≈ 2.38 and diameter 6 (small-world), versus weighted APL ≈ 17.2 and diameter 53 on the same 77-node LSCC
- Degree and strength distributions are strongly right-skewed. Weighted out-degree has the heaviest tail, with a few dominant exporters accounting for most volume
- Heavy-tail tests favour **lognormal over a pure power-law** where a verdict is reached (unweighted in-degree, p = 0.0002); the other three distributions are inconclusive, so a pure scale-free claim is not supported
- Unweighted clustering is high (avg local 0.55, transitivity 0.32) while weighted clustering is near zero (0.001 for raw quantity), so triangles exist topologically but strong trade flows are not reciprocated
### Influence & Centrality
 
- **United States:** top structural hub, ranking first on betweenness and eigenvector centrality
- **Ukraine:** highest weighted PageRank, driven largely by large post-2022 inflows from major suppliers
- Export dominance does not equal PageRank dominance. Countries such as Malaysia and Pakistan rank high through indirect connections (Katz, PageRank)
- Degree and strength are strongly correlated; betweenness and eigenvector are closely linked; Katz captures a distinct, longer-range form of influence
### Blocs & Dependency
 
- Greedy modularity yields **5 blocs** on raw quantity (Q = 0.345) but only **3 blocs** on normalized quantity (Q = 0.254). Normalization compresses the weight differences that define the blocs, so the 5-bloc result is the more informative one
- Bloc sizes are uneven: one dominant bloc of 68 countries alongside smaller regional clusters of 24–34
- A composite percentile score (out-strength, betweenness, eigenvector, Katz) places India, UAE, United States, Pakistan, and Russia at the top of the Tier 1 "high capacity" group
- HHI shows heavy single-supplier reliance in several importers, including Taiwan (HHI 0.977, 98.9% from the US), Afghanistan, Belarus, Nicaragua (Russia), and Israel
---
 
## 🛠️ Tech Stack
 
| Library | Purpose |
|---|---|
| `pandas` / `numpy` | Data loading, cleaning, imputation, and aggregation |
| `networkx` | Graph construction, centrality, path metrics, clustering, and community detection |
| `powerlaw` | Heavy-tail fitting and likelihood ratio tests |
| `scipy` | Statistical utilities and percentile ranking |
| `matplotlib` / `seaborn` | Visualization |
 
---
 
## ⚙️ Setup
 
**Prerequisites**
- Python 3.10+
**Installation**
```bash
git clone https://github.com/JasonYehezkiel/arms-trade-network-analysis.git
cd arms-trade-network-analysis
 
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate
 
pip install -r requirements.txt
```
 
Place the dataset at `datasets/global_arms_transfer_2000_2023.csv`, then run the notebook top to bottom. Sections build on one another (graphs from Section 3 and centralities from Section 7 are reused in Sections 8–10).
 
## 👤 Authored by
 
Name: Jason Yehezkiel Wijaya
 
