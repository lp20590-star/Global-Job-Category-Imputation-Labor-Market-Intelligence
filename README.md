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

Coming soon

## Example output

Coming soon

## AI Use Disclosure

## AI Use Disclosure

- **Tools used:** Google Gemini (`gemini-3.8-flash` via the `google.genai` API in Colab) and AI assistance for drafting/debugging code and documentation.
- **What I asked:** Help organizing pandas missing-category checks into a class, connecting to the Gemini API and Frankfurter currency API using Colab Secrets (`GOOGLE_API_KEY`), and fixing rate-limit/empty-country errors.
- **What I changed:** Switched the model name to `gemini-3.8-flash`, added a zero-row check in `missing_rate`, lowered the live demo sample to 5 rows with `time.sleep(4)`, and adjusted prompts and category labels to 16 standardized sectors.
- **What I wrote / verified myself:** Dataset loading from Google Drive, `unknown_by_country`, the sub-questions and country targets, `JobPostingAnalyzer` methods, and running/verifying every cell top-to-bottom on `dfall_clean.csv`.
- **Line-by-line understanding:** Yes — every cell uses MIST 4600 course Python (`for`, `if`, functions, `class`, `pandas`, `requests`, `google.genai`), and I can explain every line without AI.

## Author

Levi Michael Press

University of Georgia | Terry College of Business
