# sentiment_github_dataset

## Overview 

This repository makes the [Github Sentiment Dataset](https://figshare.com/articles/dataset/A_gold_standard_for_polarity_of_emotions_of_software_developers_in_GitHub/11604597?file=21001260) compatible with [Kaiaulu](https://github.com/sailuh/kaiaulu) for sentiment analysis of developer communication across open source software projects.

The sentiment-labeled data comes from Novielli et al. (2020), ["Can We Use SE-specific Sentiment Analysis Tools in a Cross-Platform Setting?"](https://doi.org/10.1145/3379597.3387446), and covers 7,122 GitHub comments manually labeled positive, negative, or neutral.

---

## Dataset

Download the [Github 2020 CSV](https://figshare.com/articles/dataset/A_gold_standard_for_polarity_of_emotions_of_software_developers_in_GitHub/11604597?file=21001260). The dataset contains `github_gold.csv`, which has 7,122 GitHub comments with three columns: `ID`, `Polarity`, and `Text`

The CSV alone has no author, timestamp, or project context. To recover that context, the Github 2020 CSV is joined to the [GHTorrent MSR 2014 dump](https://web.archive.org/web/20150206005357/http://ghtorrent.org/msr14.html) via `comment_id`. This yields main project repos with sentiment-labeled comments that can be downloaded and analyzed through Kaiaulu.

---

## Setup

### 1. Create and activate a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate
```

### 2. Install dependencies

```bash
pip install pandas sqlalchemy mysql-connector-python pymysql pyyaml
```

### 3. Configure notebook connection variables

Update the MySQL connection variables at the top of each notebook to match your local setup:

```python
MYSQL_HOST     = "localhost"
MYSQL_PORT     = 3306
MYSQL_USER     = "root"
MYSQL_PASSWORD = "ADD_YOUR_PASSWORD_HERE"
MYSQL_DB       = "github"
```

### 4. Configure Kaiaulu path

Clone [Kaiaulu](https://github.com/sailuh/kaiaulu) locally. Set `KAIAULU_REPO` in Notebooks 3 and 4 to your local Kaiaulu path before running.

### 5. GitHub token

A GitHub personal access token is required by Kaiaulu's download vignettes. To create one:

1. Go to **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)**
2. Enable the `public_repo` scope
3. Save the token to `~/.ssh/github_token`

---

## Notebooks

| Notebook | Description |
|---|---|
| `1_load_sentiment_csv_to_mysql.ipynb` | Loads Github 2020 CSV (`github_gold.csv`) into a local MySQL database alongside the GHTorrent dump |
| `2_explore_relevant_projects.ipynb` | Joins Gold Standard to GHTorrent via SQL to identify which main (canonical) repos have sentiment-labeled comments |
| `3_scale_config_files.ipynb` | Auto-generates one `.yml` Kaiaulu config per canonical repo. Runs Kaiaulu vignettes to download comment data |
| `4_add_sentiment_to_kaiaulu.ipynb` | Queries labels from MySQL, INNER JOINs with Kaiaulu-downloaded comment data, and writes labeled CSV output |

---

## References

- Novielli et al. (2020). ["Can We Use SE-specific Sentiment Analysis Tools in a Cross-Platform Setting?"](https://doi.org/10.1145/3379597.3387446)