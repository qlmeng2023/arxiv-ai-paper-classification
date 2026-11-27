# arXiv Computer Science Paper Classification

[English](README.md) | [简体中文](README.zh-CN.md)

## 1. Project overview

This project uses computer science papers from arXiv to classify research areas. It retrieves paper titles, abstracts, categories, authors, and publication dates through the arXiv API, then converts the text into high-dimensional sparse features with TF-IDF. The experiments compare MultinomialNB, LogisticRegression, LinearSVC, and LogisticRegression_GridSearchCV, evaluating model performance with Accuracy, macro Precision, macro Recall, and macro F1.

The selected model is LogisticRegression_GridSearchCV. The task classifies arXiv papers into five categories: cs.AI, cs.LG, cs.CV, cs.CL, and cs.SE.

## 2. Repository structure

The repository contains `code/`, `data/`, `figures/`, and `results/`. The three Python scripts are in `code/`; the figures and result artifacts are written to the corresponding directories.

<img width="491" height="467" alt="Project structure" src="https://github.com/user-attachments/assets/7a5c8e21-0c89-4529-9170-68643e961f96" />

## 3. Environment setup

Python 3.9 or later is recommended.

Option 1: install dependencies with pip

```bash
pip install requests lxml pandas numpy matplotlib scikit-learn
```

Option 2: create a virtual environment with Anaconda

```bash
conda create -n arxiv_cls python=3.10
conda activate arxiv_cls
pip install requests lxml pandas numpy matplotlib scikit-learn
```

## 4. Main dependencies

- `requests`: call the arXiv API
- `lxml`: parse XML responses from arXiv
- `pandas`: read, clean, and process tabular data
- `numpy`: numerical computation
- `matplotlib`: plot experiment figures
- `scikit-learn`: TF-IDF extraction, model training, parameter tuning, and evaluation

## 5. Data

The project collects papers in five computer science categories from the official arXiv API:

| Category | Area |
| --- | --- |
| cs.AI | Artificial intelligence |
| cs.LG | Machine learning |
| cs.CV | Computer vision |
| cs.CL | Natural language processing |
| cs.SE | Software engineering |

The repository does not include the complete generated datasets. The `data/` directory contains partial sample CSVs with descriptive Chinese filenames. Running the crawler and cleaning scripts generates `data/raw_arxiv.csv` and `data/clean_arxiv.csv`.

Run the data crawler:

```bash
python code/01_crawl_arxiv.py
```

Run the data-cleaning script:

```bash
python code/02_clean_data.py
```

Cleaning includes deduplication, missing-value handling, field organization, concatenation of titles and abstracts, and construction of auxiliary numeric features.

## 6. Run the project

From the repository root:

Step 1: install dependencies

```bash
pip install requests lxml pandas numpy matplotlib scikit-learn
```

Step 2: collect arXiv paper data

```bash
python code/01_crawl_arxiv.py
```

This script retrieves raw paper metadata, including titles, abstracts, categories, authors, and publication dates, from the arXiv API.

Step 3: clean the data and construct features

```bash
python code/02_clean_data.py
```

The script creates these fields:

| Field | Description |
| --- | --- |
| `text` | Concatenated title and abstract |
| `title_length` | Title length |
| `summary_length` | Abstract length |
| `author_count` | Number of authors |
| `category_count` | Number of category labels |
| `published_year` | Publication year |
| `published_month` | Publication month |

Step 4: train models and generate experiment results

```bash
python code/03_modeling.py
```

The script:

1. Loads the cleaned paper data.
2. Selects data for the five-category experiment.
3. Extracts text features with TF-IDF.
4. Trains MultinomialNB, LogisticRegression, and LinearSVC.
5. Tunes LogisticRegression with GridSearchCV.
6. Reports Accuracy, macro Precision, macro Recall, and macro F1.
7. Generates model-comparison plots, a confusion matrix, a learning curve, a parameter-tuning heatmap, and a feature-ablation plot.

## 7. Outputs

After running `code/03_modeling.py`, outputs are written to `figures/` and `results/`.

Figures:

- `category_distribution.png`: distribution of papers across the five categories
- `model_metrics_bar.png`: model performance comparison
- `gridsearch_heatmap.png`: GridSearchCV parameter-tuning heatmap
- `gridsearch_c_curve.png`: effect of regularization parameter C
- `learning_curve.png`: learning curve
- `confusion_matrix.png`: confusion matrix for the selected model
- `feature_ablation_impact.png`: feature-ablation analysis

Results:

- `classification_report.txt`: classification report for the selected model
- `model_metrics.csv`: Accuracy, Precision, Recall, and F1 metrics for each model
- `best_model_params.txt`: best GridSearchCV parameters
- `gridsearch_results.csv`: full grid-search results
- `feature_ablation_results.csv`: feature-ablation results
- `model_analysis_summary.txt`: model analysis summary

## 8. Models

The project compares:

- **MultinomialNB:** a Naive Bayes baseline suitable for text classification.
- **LogisticRegression:** a model suited to high-dimensional sparse TF-IDF features.
- **LinearSVC:** a linear support vector classifier for high-dimensional text classification.
- **LogisticRegression_GridSearchCV:** LogisticRegression tuned by searching the number of TF-IDF features, the n-gram range, and regularization parameter C.

## 9. Reproduce the experiments

Run the following commands in order:

```bash
pip install requests lxml pandas numpy matplotlib scikit-learn
python code/01_crawl_arxiv.py
python code/02_clean_data.py
python code/03_modeling.py
```

To avoid collecting the full dataset again, see the existing figures and results in `figures/` and `results/`.

## 10. Project notes

This repository contains code accompanying a paper for an artificial intelligence course. The source code, run instructions, and experiment outputs are provided here.

