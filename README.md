# Global-Job-Category-Imputation-Labor-Market-Intelligence

My app helps job boards identify unknown job categories using AI and determine how classifying these postings changes the most common jobs across countries.

> A course project for **MIST 4600: Computer Programming in Business** at the University of Georgia's Terry College of Business (Fall 2026).

## Problem

Nearly 47% of international job postings are labeled "unknown" or "other/general," making it difficult for job boards and recruiters to accurately identify hiring trends. My app uses AI to classify these missing categories and determine how the results change our understanding of labor markets across different countries.

## What it does

My app answers three questions:

1. **Audit:** How many postings have missing categories in each country? Germany has 86.8% missing, while the USA and UK have almost none.
2. **LLM Check:** How often does Gemini match existing labels, and what jobs were hidden? The baseline exact match rate is 43%, revealing jobs in Trades, Finance, Healthcare, and Sales.
3. **Shifts & Action:** How do the most common jobs change by country after classification, and how can job boards improve recruitment and advertising decisions?

## Data

The dataset comes from **Kaggle** and contains 4,000 job postings across eight countries: USA, UK, Germany, France, Netherlands, Italy, Spain, and Switzerland (500 per country).

Each row represents one job posting. The primary unstructured data consists of multilingual job titles, which are analyzed using AI to classify missing job categories.

Dataset link: Coming soon

## Built with

- Python and pandas in Google Colab
- Google Gemini AI for job classification

## How to run it
1. **Get the data:** Use `dfall_clean.csv` (4,000 international Adzuna job postings from Kaggle's Global Job Market & Salary dataset, or the included 20-row `sample_rows__3__629d0f3b (2).csv` test file in this repository).
2. **Put the data in Google Drive:** Upload `dfall_clean.csv` to your Google Drive (for example, inside `MyDrive/MIST 4600/`). Cell 1 mounts Google Drive (`drive.mount("/content/drive")`) and automatically finds and loads the CSV.
3. **Open the notebook:** Open `Global_Job_Category_Imputation_Labor_Market_Intelligence.ipynb` in Google Colab.
4. **Add your API key in Colab Secrets (never type a key into the code):**
   - Click the **Key icon (Secrets)** on the left sidebar in Google Colab.
   - Add a secret named **`GOOGLE_API_KEY`** with your Google AI Studio Gemini key.
   - Toggle **Notebook access** to **ON** (Cell 4 reads it safely using `userdata.get("GOOGLE_API_KEY")`).
5. **Run from top to bottom:** Click **Runtime → Run all** and approve the Google Drive prompt. Cells 1–8 run in order (pausing 4 seconds per Gemini call to stay under free-tier rate limits) and save the classified sample to `classified_jobs.csv`.

## Example output
```text
Loaded 4000 jobs from: /content/drive/MyDrive/MIST 4600/dfall_clean.csv

country
France         164
Germany        411
Italy           13
Netherlands    253
Spain          254
Switzerland     13
dtype: int64

Germany: 86.8% missing
Switzerland: 71.0% missing
France: 65.0% missing
Spain: 64.6% missing
Netherlands: 56.2% missing
Italy: 31.8% missing
United Kingdom: 0.4% missing
USA: 0.0% missing

Mean salary by country:
country
France            123224.470
Germany            23114.742
Italy              19035.106
Netherlands        34016.826
Spain               7019.120
Switzerland        31469.002
USA                74163.670
United Kingdom     39405.110

=== FINAL RECOMMENDATION ===
1. Biggest blind spot: Germany at 86.8% missing categories.
2. Priority markets (15%+ unlabeled): Germany (86.8%), Switzerland (71.0%), France (65.0%), Spain (64.6%), Netherlands (56.2%), Italy (31.8%)
3. Action: Use Gemini auto-classification on unlabeled titles before running search ads and recruiter packages.

## AI Use Disclosure

- **Tools used:** Google Gemini (`gemini-3.8-flash` via the `google.genai` API in Colab) and AI assistance for drafting/debugging code and documentation.
- **What I asked:** Help organizing pandas missing-category checks into a class, connecting to the Gemini API and Frankfurter currency API using Colab Secrets (`GOOGLE_API_KEY`), and fixing rate-limit/empty-country errors.
- **What I changed:** Switched the model name to `gemini-3.8-flash`, added a zero-row check in `missing_rate`, lowered the live demo sample to 5 rows with `time.sleep(4)`, and adjusted prompts and category labels to 16 standardized sectors.
- **What I wrote / verified myself:** Dataset loading from Google Drive, `unknown_by_country`, the sub-questions and country targets, `JobPostingAnalyzer` methods, and running/verifying every cell top-to-bottom on `dfall_clean.csv`.
- **Line-by-line understanding:** Yes — every cell uses MIST 4600 course Python (`for`, `if`, functions, `class`, `pandas`, `requests`, `google.genai`), and I can explain every line without AI.

## Author

Levi Michael Press

University of Georgia | Terry College of Business
