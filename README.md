# Global Job Category Imputation and Labor Market Intelligence

My app helps **job boards** decide **where to repair missing job categories and which sector campaigns to test** by comparing job postings across eight countries.

## Problem

In the included 4,000-posting sample, 1,879 original job categories (47.0%) are `unknown` or `other-general-jobs`. That makes the postings harder to organize. Each country contributes 500 postings, so these counts describe this sample, not the size of a country's labor market.

## What it does

Open [unknown_jobs.ipynb](unknown_jobs.ipynb) in Colab. The notebook answers three questions with pandas, one function per question:

1. **Q1 · Audit:** Which countries have the most missing categories? `question_1` counts the original missing tags and calculates a percentage for each country.
2. **Q2 · LLM check:** How often did Gemini labels match existing sectors, and what types were hidden in missing categories? `question_2` displays the earlier completed Gemini run's aggregate results with pandas. `JobChatbot.label_one()` also calls Gemini on one originally missing title to show a live example of the same classification task.
3. **Q3 · Shifts (my own idea):** How did the top sectors in each country change after filling missing categories? `question_3` displays before, after, and change with pandas.

The notebook includes the full original [dfall_clean.csv](dfall_clean.csv) and the public, title-free [completed_results.csv](completed_results.csv). The aggregate file was calculated from an earlier Gemini classification of all 4,000 titles; **Q2 and Q3 use those saved counts**, so Run all does not repeat 4,000 API calls. The live example is separate and does not change those historical counts. The row-level classified file remains private.

## How to run it

1. Open the [notebook in Colab](https://colab.research.google.com/github/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/main/unknown_jobs.ipynb).
2. In Colab **Secrets**, add `GOOGLE_API_KEY` and enable **Notebook access**. The key is never typed into the notebook or repository. This is needed for the live Gemini example and the outside-tool rubric item.
3. Select **Runtime → Run all**. The notebook installs `google-genai`, downloads both public CSVs from this repository, calls Gemini once, and prints the three answers and a decision. No Google Drive mount or file upload is required.

If the secret or API is unavailable, the live example prints an error message and the saved Q1–Q3 answers still run. A successful live Gemini call is required to demonstrate the outside-tool criterion. The `gemini-3.5-flash-lite` model and `google-genai` package may depend on the availability and limits of your API account.

## Example output

These are real results from the included CSVs. The live Gemini example varies with the API response.

```text
Postings loaded: 4000
Q1 · Missing categories by country
          country  postings  missing  percent
0         Germany       500      434     86.8
1     Switzerland       500      355     71.0
2          France       500      325     65.0
3           Spain       500      323     64.6
4     Netherlands       500      281     56.2
5           Italy       500      159     31.8
6  United Kingdom       500        2      0.4
7             USA       500        0      0.0

Q2 · Agreement and hidden sectors
Agreement: 1355 of 2091 = 64.8 %
Missing categories filled: 1876 of 1879
                              sector  count
1               trade & construction    407
2   logistics, driver & supply chain    227
3                        engineering    204
4      accounting, banking & finance    176

Q3 · Top sectors after filling missing categories
           country                            sector  before  after  change
20         Germany                       engineering       6     81      75
21         Germany              healthcare & nursing       9     73      64
18          France     accounting, banking & finance       3    105     102
32     Switzerland              trade & construction      65    162      97

Decision: Repair categories in Germany first; test engineering campaigns there.
```

The full output also lists the other hidden sectors and top three sectors for all eight countries. The 2,091 agreement denominator excludes two uncertain AI labels among 2,093 comparable source labels. Three of 1,879 originally missing rows remained uncertain. Agreement with an existing tag is **not verified accuracy**. Before/after changes are reclassification of sampled postings, **not employment growth**.

## AI Use Disclosure

AI assistance helped simplify and review the notebook and this README. Gemini labeled the job titles in the earlier completed run and is called for one live example in the notebook. The pandas counts were checked against the included original CSV and public aggregate file. The user should be able to explain each cell and formula on the exam.

## Optional Streamlit app and Honors report

The [deployed Streamlit app](https://global-job-category-imputation-labor-market-intelligence-gcul5.streamlit.app/) is an optional interface. Its API features require a separate Streamlit secret; its code is separate from the Colab notebook. The Honors Option also requires a separate 3–5 page report submitted through eLC.

Levi Michael Press · University of Georgia
