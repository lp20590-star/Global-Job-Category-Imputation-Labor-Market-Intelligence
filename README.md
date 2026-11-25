# Global Job Category Imputation and Labor Market Intelligence

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/main/unknown_jobs.ipynb)

## Problem

My app helps a job board manager decide which country and sector to target for a first category-repair campaign by comparing missing tags and AI-labeled job titles across eight countries.

A job seeker who filters for engineering can miss an engineering role if its category is blank or too general. In this sample of 4,000 Adzuna postings, 1,879 (47.0%) have the source tag `unknown` or `other-general-jobs`. There are 500 postings in each of eight countries. Each row describes one posting and includes its country, job title and original category.

## What it does

| Question | Notebook function | Output |
| --- | --- | --- |
| Q1. Where are the most categories missing? | `missing_by_country` | Adds the two missing-tag counts, calculates the percentage in each country, and finds the highest share. |
| Q2. How often did Gemini agree with comparable tags, and which sectors were hidden? | `agreement_and_hidden` | Calculates agreement, lists all 16 hidden-sector counts and shows the current AI label for one UK title. |
| Q3. Which sectors lead after repair, and where should the manager run a pilot? **My original sub-question.** | `sector_shifts` | Calculates changes for the top three sectors in each country, uses the UK example to measure the effect of one repair, and recommends a country and sector. |

The full classifier sent job titles and countries to Gemini and recorded an `ai_type` sector or `uncertain`. Q2 uses that column to measure agreement and count previously hidden sectors. Q3 keeps the known original sectors and adds valid AI labels for missing ones. The downloadable results contain total counts from the completed run. The full classifier and its prompt are in [app.py](app.py).

The notebook uses one small [JSON file of public counts](project_counts.json). Its audit table counts the two original missing tags separately for each country in [dfall_clean.csv](dfall_clean.csv). Its agreement counts, 16 hidden-sector counts and 24 ranking rows come from [completed_results.csv](completed_results.csv). Each country’s three ranking rows are ordered by final count, with ties in alphabetical order. The notebook calculates missing shares, agreement, coverage and changes from these numbers.

The `JobChatbot` class answers from the calculated results. In default mode, keywords retrieve the missing-tag audit, agreement, hidden sectors, changes or pilot recommendation. In live mode, Gemini answers free-text questions using those same results. The one-title example uses a recorded Gemini answer in default mode. In live mode, `label_title` calls Gemini and validates a fresh sector before Q2 and Q3 use it. [Earlier Colab run with the recorded Gemini response](https://github.com/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/a0b8be558dab319dcd299b067ad72a16af3b58dc/unknown_jobs.ipynb)

## How to run it

1. Click **Open in Colab** at the top of this page or [open the notebook directly](https://colab.research.google.com/github/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/main/unknown_jobs.ipynb).
2. Leave `RUN_LIVE = False` in section 1 and choose **Runtime → Run all**. Requests downloads the public JSON from this repository. The default needs internet access and runs without a personal key or file upload.
3. To chat, edit the quoted `question` in the **last code cell** and press **Shift + Enter**. Try “How often did Gemini agree?”, “Which sectors were hidden?”, or “Where should we run the pilot?” Each asks for a different calculated answer.
4. For **fresh Gemini classification and open-ended chat**, enable notebook access to `GOOGLE_API_KEY` in Colab Secrets, change `RUN_LIVE` to `True` and run all once. This makes two Gemini calls, one for the UK title and one for the question. Further questions only require running the last cell.

## Example output

These selected lines come from running the notebook with its default settings.

```text
Postings in the sample: 4000
Highest missing share: Germany.
          country  postings  unknown  other_general  missing  percent
0         Germany       500      411             23      434     86.8

Agreement: 1355/2091 (64.8%). This compares Gemini with existing tags.
Missing categories filled: 1876/1879.
                              sector  count
0               trade & construction    407
1   logistics, driver & supply chain    227
2                        engineering    204
UK title check: Teaching Assistant → teaching & education

Germany
                 sector  before  after  gain
0           engineering       6     81    75
UK one-title scenario: teaching & education 103 → 104.
Pilot category repair in Germany and test a campaign for engineering after label review.
```

**How to read these numbers.** Of 2,121 originally known tags, 28 fell outside the 16-sector mapping and two received uncertain AI labels. That leaves 2,091 pairs for the agreement calculation. Three of the 1,879 missing tags remained uncertain. Existing tags provide a comparison point. An independent human review would be needed to measure accuracy. Sector gains count repaired tags in these same postings.

## AI Use Disclosure

I used ChatGPT/Codex to generate the Python code and help write this README. I asked it to simplify the cells, shorten the time needed to run the project, debug the notebook and check the results against the course rubric. I requested revisions to use the course cheat sheets and keep the chatbot question. AI generated the code, including the revisions. My own contribution was directing those changes and reviewing the code line by line to make sure I understood what it does. Gemini was the project’s outside tool for assigning sectors to job titles and answering live questions.

## Optional Streamlit app

Open the [live Streamlit app](https://global-job-category-imputation-labor-market-intelligence-gcul5.streamlit.app/). It shows the original category audit and can classify batches of job titles. Use the **Ask the data** tab to type a question and press **Enter**. The app uses its own deployment secret. The notebook above is the course submission entry point.

Levi Michael Press · University of Georgia
