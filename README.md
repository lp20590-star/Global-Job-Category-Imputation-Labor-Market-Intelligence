# Global Job Category Imputation and Labor Market Intelligence

My app helps **job boards** decide **where to repair missing job categories and which sector campaigns to test** by labeling job titles and comparing sectors across eight countries.

> MIST 4600 Honors Option · University of Georgia · Fall 2026

## Problem

Job boards cannot reliably sort or market postings when their category is missing. In this dataset, **1,879 of 4,000 postings (47.0%)** have an original tag of unknown or other-general-jobs. Each country has 500 sampled postings, so these results describe this dataset, not the size of a country's labor market.

## What it does

The notebook answers three connected questions with pandas:

1. **Audit:** How many postings have missing categories in each country? This identifies where category repair matters most.
2. **LLM check:** How often does Gemini's title-based sector label agree with an existing mapped sector? Which sectors does it find among originally missing rows? Agreement with an existing tag is a check, not proof that either label is correct.
3. **Shifts and action:** How do sector counts change after filling missing labels? The app recommends repairing categories first in the country with the highest original missing share, then testing sector-specific recruiter and advertising campaigns.

Gemini turns the free-text job title into the new ai_type column. That column is used in questions 2 and 3. Existing known sectors stay unchanged in the before/after comparison. The class JobChatbot holds the jobs, classifies titles in batches, and answers questions using the calculated tables.

**Comparison rule:** The CSV has 2,121 rows with a nonmissing category. Of these, **2,093** map to the 16 chosen job sectors. The other 28 have part-time, graduate, or charity/voluntary tags. They are shown as outside taxonomy and excluded from exact sector agreement. The agreement denominator counts only those of the 2,093 that Gemini actually labeled with a sector; uncertain labels are excluded and should be reported separately.

## Data and tools

- dfall_clean.csv is included in this repository: 4,000 Adzuna job postings, 500 each from France, Germany, Italy, the Netherlands, Spain, Switzerland, the United Kingdom, and the USA. The job title is the unstructured input. Add the exact original Kaggle dataset URL here when confirmed.
- Python and pandas in Google Colab calculate all counts, percentages, and comparisons.
- Google Gemini is the outside API that reads multilingual titles and supplies the ai_type sector label. The notebook uses the google-genai library and model gemini-3.5-flash-lite.
- app.py is the optional Streamlit interface.

## How to run it

1. Download the included full dfall_clean.csv and upload **one** file with that exact name anywhere inside Google Drive's MyDrive. The 20-row sample is not the project dataset.
2. Open [Unknown_jobs.ipynb](Unknown_jobs.ipynb) in Google Colab.
3. To use the completed results from the November 24 Gemini run, put your private classified_jobs.csv beside dfall_clean.csv **in your Drive** before running the notebook. The notebook checks the source rows and loads all 4,000 labels. This file is available privately to the author; it is not in the public repository.
4. Select **Runtime → Run all** and authorize Google Drive. Q1–Q3 use the saved results without an API key. The last cell needs an API key to ask Gemini a question.
5. To classify from scratch or ask a question, add GOOGLE_API_KEY in Colab **Secrets** using your personal Google AI Studio API key, and turn **Notebook access** on. Never put a key in the notebook, README, or repository. By default, Part 4 classifies one batch of up to 20 titles per run. Rerun Part 4 to continue, or set BATCHES = 200 for all remaining titles in one run. A full run can take many minutes and may stop at an API limit; completed batches are saved in Drive for the next run.
6. The three outputs appear under Q1, Q2, and Q3. The last cell optionally asks Gemini to explain those calculated tables. Without the saved labels, Q2 and Q3 are provisional until all titles have been processed; the cell prints the number still pending.

The notebook does not assume that a partial batch or an uncertain prediction is a valid filled label. It checks every row and allowed sector before saving a batch. If the source CSV changes, rename the older classified_jobs.csv before rerunning.

## Example output

The following Q1 numbers were recalculated from the included 4,000-row CSV:

| Country | Postings | Missing | Missing % |
| --- | ---: | ---: | ---: |
| Germany | 500 | 434 | 86.8% |
| Switzerland | 500 | 355 | 71.0% |
| France | 500 | 325 | 65.0% |
| Spain | 500 | 323 | 64.6% |
| Netherlands | 500 | 281 | 56.2% |
| Italy | 500 | 159 | 31.8% |
| United Kingdom | 500 | 2 | 0.4% |
| USA | 500 | 0 | 0.0% |

**Q2 · Completed Gemini run (November 24):** 4,000 titles were processed. Gemini assigned a sector to 3,995 and marked 5 uncertain. Among the 2,093 source tags that map to the 16 sectors, 2,091 received an AI sector; **1,355 of those 2,091 matched (64.8%)**. Gemini filled **1,876 of 1,879** originally missing categories; the other 3 were uncertain. The most common sectors hidden in missing categories were Trades & Construction (407), Logistics & Supply Chain (227), Engineering (204), Finance (176), Sales (157), and Healthcare (132). These results supersede the unsupported earlier figure of 913/2,121 (43%). Agreement with an existing source tag is not verified accuracy.

**Q3 · Selected country changes (original sector count → after labeling):**

| Country | Sector | Before | After |
| --- | --- | ---: | ---: |
| Germany | Engineering | 6 | 81 |
| Germany | Healthcare | 9 | 73 |
| Germany | Trades & Construction | 0 | 67 |
| France | Healthcare | 128 | 156 |
| France | Finance | 3 | 105 |
| Switzerland | Trades & Construction | 65 | 162 |
| Spain | Trades & Construction | 0 | 114 |

**Recommendation:** Prioritize category repair in Germany, where 434 of 500 source categories were missing. After labeling, engineering leads its specific sectors in this sample; test an engineering recruiter plan or targeted campaign there. Three missing rows remain uncertain. The before/after changes are reclassifications of sampled postings, not employment growth.

## Exam guide

- **Load:** pd.read_csv reads the complete CSV. A row is one posting; the title is the unstructured input.
- **Missing flag:** isin checks whether a source tag is unknown or other-general-jobs.
- **Q1:** groupby country counts all rows and sums True missing flags. The percentage is missing divided by postings times 100.
- **Gemini:** classify sends up to 20 indexed titles per batch, accepts only one allowed label per index, writes ai_type, and saves after each valid batch. BATCHES controls how many batches Part 4 processes per run. The class keeps the DataFrame in self.jobs.
- **Q2:** Filter known mapped sectors with an AI label; compare before_type to ai_type. Count newly identified sectors only in rows originally missing.
- **Q3:** Copy before_type to after_type and replace it with ai_type only where the category was originally missing and Gemini returned an allowed sector. Count sectors by country before and after. A change reflects relabeling of sampled postings, not growth in employment.
- **Chat:** ask passes the pandas summaries to Gemini and asks it to explain the evidence. Python does all counting; Gemini reads titles and writes prose.

## AI Use Disclosure

AI assistance was used to review the rubric, simplify and debug the notebook, and draft documentation. The code was checked against the included 4,000-row CSV and a simulated API response. On November 24, Gemini classified all 4,000 titles through the deployed Streamlit app using the same 16-sector taxonomy; the resulting labels were checked against the source CSV and the notebook's pandas calculations. The notebook-compatible result file is kept private. The author should review the labels, run the notebook with that file in Drive, and adjust this disclosure to accurately describe their personal work and understanding.

## Optional Streamlit web app

[Open the deployed Streamlit app](https://global-job-category-imputation-labor-market-intelligence-gcul5.streamlit.app/). Its sidebar can classify pending titles in batches and its tabs show the audit, AI labels, sector changes, and a data question. App predictions last for the current browser session; the Colab notebook saves its own classified_jobs.csv in Drive. The deployment's API features require GOOGLE_API_KEY in Streamlit secrets.

## Honors report

The Honors Option also requires a separate 3–5 page report with Question and data, How you built the new column, What you found, Business implications, and Limitations. Submit that report and this repository link through the appropriate eLC dropboxes by the course deadline.

## Author

Levi Michael Press · University of Georgia, Terry College of Business
