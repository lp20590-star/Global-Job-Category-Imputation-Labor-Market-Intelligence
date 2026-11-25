# Global Job Category Imputation and Labor Market Intelligence

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/main/unknown_jobs.ipynb)

## Problem

My app helps a job board manager decide which country and sector to target for a first category-repair campaign by comparing missing tags and AI-labeled job titles across eight countries.

A job seeker who filters for engineering can miss an engineering role if its category is blank or too general. In this sample of 4,000 Adzuna postings, 1,879 (47.0%) have the source tag `unknown` or `other-general-jobs`. There are 500 postings in each of eight countries. Each row describes one posting and includes its country, job title and original category.

## What it does

| Question | Notebook function | Output |
| --- | --- | --- |
| Q1. Where are the most categories missing? | `missing_by_country` | Counts missing tags and calculates their share of each country’s postings. |
| Q2. How often did Gemini agree with comparable tags, and which sectors were hidden? | `agreement_and_hidden` | Calculates agreement, lists all 16 hidden-sector counts and shows the current AI label for one UK title. |
| Q3. Which sectors lead after repair, and where should the manager run a pilot? **My original sub-question.** | `sector_shifts` | Calculates changes for the top three sectors in each country, uses the UK example to measure the effect of one repair, and recommends a country and sector. |

The full classifier sent job titles and countries to Gemini and recorded an `ai_type` sector or `uncertain`. Q2 uses that column to measure agreement and count previously hidden sectors. Q3 keeps the known original sectors and adds valid AI labels for missing ones. The downloadable results contain total counts from the completed run. The full classifier and its prompt are in [app.py](app.py).

The notebook’s `JobChatbot` class validates a sector, labels a title and answers free-text questions about the printed results. A recorded example from an earlier Colab run makes the default demonstration reproducible. Switching on live mode replaces the example’s label before Q2 and Q3 run. [Earlier Colab run with the real Gemini response](https://github.com/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/a0b8be558dab319dcd299b067ad72a16af3b58dc/unknown_jobs.ipynb)

## How to run it

1. Open [unknown_jobs.ipynb in Google Colab](https://colab.research.google.com/github/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/main/unknown_jobs.ipynb).
2. Leave `RUN_LIVE = False` and select **Runtime → Run all**. Requests downloads [dfall_clean.csv](dfall_clean.csv) and [completed_results.csv](completed_results.csv) directly from this public repository. The notebook prints all three answers and the pilot decision. The default needs internet access and runs without a key, a Drive mount or a file upload.
3. For **fresh Gemini output**, open **Secrets** in Colab, add `GOOGLE_API_KEY` and enable notebook access. Set `RUN_LIVE = True` and select **Runtime → Run all**. There are two Gemini calls, one for the UK title and one for the final chatbot question. To ask something else, edit the question in the last code cell and rerun that cell. The code reads the key from Colab Secrets.

## Example output

These selected lines come from running the notebook with its default settings.

```text
Postings loaded: 4000
Germany: 434/500 (86.8%)
USA: 0/500 (0.0%)
Agreement: 1355/2091 (64.8%)
Missing categories filled: 1876/1879
trade & construction: 407
logistics, driver & supply chain: 227
engineering: 204
UK title check: Teaching Assistant → teaching & education
Germany / engineering: 6 → 81 (+75)
Switzerland / trade & construction: 65 → 162 (+97)
United Kingdom / teaching & education: 103 → 105 (+2)
UK one-title scenario: education 103 → 104 (+1)
Decision: pilot category repair in Germany and test a campaign for engineering after a label review.
```

**How to read these numbers.** Of 2,121 originally known tags, 28 fell outside the 16-sector mapping and two received uncertain AI labels. That leaves 2,091 pairs for the agreement calculation. Three of the 1,879 missing tags remained uncertain. Existing tags provide a comparison point. An independent human review would be needed to measure accuracy. Sector gains count repaired tags in these same postings.

## AI Use Disclosure

I used ChatGPT/Codex to generate the Python code and help write this README. I asked it to simplify the cells, shorten the time needed to run the project, debug the notebook and check the results against the course rubric. I requested revisions to use the course cheat sheets and keep the chatbot question. AI generated the code, including the revisions. My own contribution was directing those changes and reviewing the code line by line to make sure I understood what it does. Gemini was the project’s outside tool for assigning sectors to job titles and answering live questions.

## Optional Streamlit app

Open the [live Streamlit app](https://global-job-category-imputation-labor-market-intelligence-gcul5.streamlit.app/). It shows the original category audit, classifies a batch of job titles and updates the assigned and pending counts. The app uses its own deployment secret. The notebook above is the course submission entry point.

Levi Michael Press · University of Georgia
