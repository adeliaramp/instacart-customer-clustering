# Customer Segmentation Analysis  
## Instacart Market Basket Data

## Project Overview

This project explores **customer behavior in grocery e-commerce** by segmenting users based on their purchasing patterns. Using the **Instacart Market Basket dataset** (3M+ orders, 200K+ users), I identify distinct shopper personas that can inform **personalized marketing, inventory planning, and retention strategies**.

**Note:**  
This is an exploratory analysis using open-source data. While Instacart likely operates more advanced internal systems, this project demonstrates a **practical, resource-efficient clustering workflow** applicable to small-to-medium grocery delivery businesses or retailers without dedicated ML teams.

---

## Business Application

### Target Audience
- Grocery delivery startups  
- Regional supermarket chains expanding into e-commerce  
- Businesses without mature data science or ML infrastructure  

### Potential Use Cases
- Personalized email and promotion campaigns  
- Inventory and demand planning by customer segment  
- Retention strategies for high-value customers  
- UX customization based on shopping behavior  
- Delivery window and logistics optimization  

---

## Research Questions

- Can we identify meaningful shopper personas using transaction data alone?  
- Which behavioral features best differentiate customers?  
- How stable and interpretable are the resulting segments?  
- What actionable insights can be translated into business strategy?  

---

## Dataset

**Source:** Kaggle – *Instacart Market Basket Analysis*

### Scale
- 3+ million grocery orders  
- 200,000+ users  
- 50,000+ products  
- 134 aisles, 21 departments  

### Key Files
- `orders.csv` – Order metadata and timing  
- `order_products.csv` – Products within each order  
- `products.csv` – Product attributes and aisle/department mapping  
- `aisles.csv`, `departments.csv` – Category hierarchies  

---

## Methodology

### 1. Feature Engineering

User-level features were created across four dimensions.

#### Product Preferences
- Purchase ratio per aisle (134 features)  
- Department diversity score  
- Organic product ratio  

#### Shopping Habits
- Average basket size  
- Order frequency (days between orders)  
- Basket composition consistency  

#### Temporal Patterns
- Preferred order hour  
- Weekday vs weekend tendency  
- Order regularity (coefficient of variation)  

#### Loyalty Indicators
- Reorder ratio  
- Average product reorder frequency  
- Product / brand loyalty score  

---

### 2. Preprocessing
- Excluded users with fewer than **5 orders** (insufficient behavioral signal)  
- Feature normalization using `StandardScaler`  
- Dimensionality reduction with **PCA**  
  - 134 → 40 components  
  - ~95% variance retained  

---

### 3. Clustering Approach

#### Algorithms Evaluated
- K-Means (baseline, scalable)  
- Hierarchical clustering (structure exploration)  
- DBSCAN (outlier detection, non-spherical clusters)  

#### Choosing the Number of Clusters
- Elbow method (WCSS)  
- Silhouette analysis  
- Business interpretability (manageable segment count)  

#### Final Choice
- **K-Means with K = 5**, balancing performance and actionability  

---

### 4. Validation
- Silhouette score per cluster  
- Stability checks across random seeds  
- Business logic validation (intuitive segment behavior)  
- Feature importance analysis  

---

## Key Results

### Five Distinct Customer Segments

#### Segment 1: Health-Conscious Regulars (18%)
- High fresh produce and organic purchases  
- Weekly ordering cadence (avg 7.2 days)  
- Morning shoppers (08:00–11:00)  
- Medium baskets, high reorder ratio (72%)  

**Business Action:** Healthy recipes, organic promotions, wellness content  

---

#### Segment 2: Bulk Pantry Stockers (24%)
- Packaged goods, canned food, household supplies  
- Monthly orders (avg 28 days)  
- Very large baskets (30+ items)  

**Business Action:** Bulk discounts, subscriptions, stock-up reminders  

---

#### Segment 3: Busy Convenience Seekers (15%)
- Prepared foods, frozen meals, snacks  
- Irregular ordering, late-night activity  
- Small to medium baskets, low reorder ratio (45%)  

**Business Action:** Quick re-order UX, meal kits, late-night delivery slots  

---

#### Segment 4: Family Shoppers (28%)
- Broad category coverage (dairy, bread, meat, snacks, baby items)  
- Bi-weekly orders (avg 14 days)  
- Large baskets (20–25 items)  

**Business Action:** Family bundles, kid-friendly promotions  

---

#### Segment 5: Exploratory Foodies (15%)
- High category diversity, specialty and international products  
- Weekend preference, low reorder ratio (38%)  
- Medium baskets, frequent experimentation  

**Business Action:** New product alerts, curated discovery feeds  

---

## Model Performance

- **Silhouette Score:** 0.42 (typical for behavioral clustering)  
- **WCSS Reduction:** 68% from K = 1 → K = 5  
- **Cluster Sizes:** Balanced (15–28%), no dominant segment  

---

## Feature Importance

Top differentiating features:
- Fresh produce purchase ratio  
- Order frequency  
- Reorder ratio  
- Packaged goods ratio  
- Order time preference  

---

## Technical Implementation

### Tech Stack
- Python 3.9+  
- pandas, numpy  
- scikit-learn (`KMeans`, `AgglomerativeClustering`, `DBSCAN`)  
- PCA, UMAP  
- matplotlib, seaborn, plotly  

---

## How to Reproduce

### 1. Clone the Repository
Clone this repository to your local machine and navigate into the project directory.

---

### 2. Set Up the Environment
Install the required Python dependencies listed in `requirements.txt`.

Ensure that you have a valid **Kaggle API configuration**:
- Create a Kaggle account if you do not already have one  
- Generate an API token from your Kaggle account settings  
- Place the `kaggle.json` file in the default Kaggle configuration directory:
  - `~/.kaggle/` for macOS and Linux  
  - `%USERPROFILE%\.kaggle\` for Windows  

---

### 3. Download the Dataset Using kagglehub
This project uses **kagglehub** to download the Instacart Market Basket dataset programmatically.

When you run the notebooks:
- The dataset is fetched automatically via the Kaggle API  
- Files are cached locally by kagglehub  
- No manual CSV downloads or file placement are required  

The notebooks access the dataset directly from kagglehub’s cache, ensuring consistent paths and reproducibility across environments.

---

### 4. Run the Analysis
Run the notebooks in sequential order:

1. `01_eda.ipynb` – Exploratory data analysis  
2. `02_feature_engineering.ipynb` – Feature construction and preprocessing  
3. `03_clustering.ipynb` – Customer segmentation and model selection  
4. `04_validation.ipynb` – Cluster evaluation and stability checks  

Each notebook depends on artifacts generated in the previous step.

---

### Notes
- The dataset is downloaded only once and reused from cache in subsequent runs  
- All data paths are handled programmatically to avoid environment-specific issues  
- The workflow is compatible with local machines, cloud notebooks, and CI environments  

---

## Key Visualizations

- **UMAP Cluster Distribution**  
  2D projection showing separation and overlap between customer segments  

- **Silhouette Analysis**  
  Evaluation of intra-cluster cohesion and inter-cluster separation  

- **Segment Profile Radar Charts**  
  Comparison of normalized feature means across segments  

- **Feature Importance Plots**  
  Identification of behaviors that most strongly differentiate clusters  

- **Temporal Ordering Heatmaps**  
  Order timing patterns by hour and day across segments  

---

## Insights & Learnings

### What Worked
- PCA significantly improved clustering quality by reducing noise from high-dimensional aisle data  
- Combining product preferences, temporal behavior, and loyalty signals created more distinct segments  
- Reorder ratio emerged as a strong and intuitive behavioral discriminator  

### Challenges
- Natural class imbalance across behavioral segments  
- Trade-off between clustering metrics and business interpretability  
- Lack of demographic and pricing data limited deeper personalization  

---

## Future Work
- Experiment with **Gaussian Mixture Models** for soft cluster assignment  
- Apply **time-series or sequence-based clustering** instead of aggregated features  
- Validate segments using **A/B testing** in real campaigns  
- Incorporate **pricing and basket value** for value-based segmentation  
- Explore **real-time user classification** with streaming data  

---

## Limitations
- Unsupervised clusters are mathematical constructs, not ground truth  
- Dataset reflects **pre-pandemic** shopping behavior  
- Limited external validity beyond Instacart users  
- Results depend on feature engineering choices  
- No causal claims are made  

---

## Contact & Feedback

**[Adelia Ramadhani Putri]**  
[[LinkedIn](https://www.linkedin.com/in/adeliaramp/)] | [[GitHub](https://github.com/adeliaramp)] | [adeliaramp@gmail.com]  

Feedback and collaboration ideas are welcome.

---

## Acknowledgments
- Instacart and Kaggle for releasing the dataset  
- Referenced tutorials, articles, and research materials  

---

## License
This project is for **educational and portfolio purposes only**.  
Dataset usage is subject to Instacart and Kaggle terms.
