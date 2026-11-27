# Job Category Chatbot

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/main/unknown_jobs.ipynb)

## Problem

My app helps a job board manager decide which country and sector to target for a first category-repair campaign by comparing missing tags and AI-labeled job titles across eight countries.

A job seeker filtering for engineering can miss a relevant posting when its category is blank or too general. The dataset contains 4,000 Adzuna job postings, 500 in each of eight countries. Each row has a country, a free-text job title and an original category. I count the source tags `unknown` and `other-general-jobs` as missing. [Original postings](dfall_clean.csv)

## What it does

| Sub-question | Function | What the manager sees |
| --- | --- | --- |
| 1. Where do job postings have the most missing categories? | `missing_by_country` | A pandas table of counts and percentages for all eight countries. Germany has the highest missing share, 434 of 500 postings (86.8%). |
| 2. How often did Gemini agree with comparable tags, and which sectors were hidden? | `agreement_and_hidden` | Agreement is 64.8% across 2,091 comparisons. The table shows all 16 hidden sectors. Trade and construction leads with 407 postings. |
| 3. Which sectors lead after repair, and where should the manager run a pilot? **My original sub-question.** | `sector_shifts` | A before-and-after table for each country’s three leading sectors. The pilot recommendation is Germany, where engineering rises from 6 to 81 postings after repair. |

**How Gemini contributes.** The full classifier sent job titles and countries to Gemini and saved each returned sector in a new `ai_type` column. Q2 uses the resulting counts to compare AI labels with existing tags and identify hidden sectors. Q3 uses the repaired counts to recommend a pilot. The notebook also demonstrates both uses with one UK title, `Teaching Assistant`. Q2 displays its `ai_type`. Q3 checks whether that label adds one posting to teaching and education. [Full classifier](app.py) · [Recorded Gemini example](https://github.com/lp20590-star/Global-Job-Category-Imputation-Labor-Market-Intelligence/blob/a0b8be558dab319dcd299b067ad72a16af3b58dc/unknown_jobs.ipynb)

`JobChatbot` stores the computed answers in `__init__`. The last cell calls its `ask` method to answer the question you type.

## How to run it

1. Click **Open in Colab** above. Leave `RUN_LIVE = False` in section 1 and choose **Runtime → Run all**.
2. The first code cell uses the `requests` package to download `project_counts.json` from this public repository. It contains original country counts and totals from the completed Gemini run. No file upload or personal key is needed for this mode. [Notebook input](project_counts.json) · [Completed Gemini totals](completed_results.csv)
3. To chat, change the quoted `question` in the **last code cell** and press **Shift + Enter**. Try “How often did Gemini agree?”, “What sectors were hidden?” or “Where should we run the pilot?”
4. For **live Gemini**, give the notebook access to `GOOGLE_API_KEY` in Colab **Secrets**, set `RUN_LIVE = True` and run all again. Gemini classifies the UK example and answers the chat question using the calculated results. For each new question, rerun only the last cell. The eight-country analysis uses the completed run in both modes.

**Optional Streamlit app.** Open **Ask the data** and enter “Which country has the most missing categories?” The live app starts each session with the original postings. Classifying batches updates that session’s results. [Open the Streamlit chatbot](https://global-job-category-imputation-labor-market-intelligence-gcul5.streamlit.app/)

## Example output

Selected lines from a default **Run all**, in the order the notebook prints them.

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

Pilot category repair in Germany and test a campaign for engineering after label review.
Germany
                 sector  before  after  gain
0           engineering       6     81    75
UK one-title scenario: teaching & education 103 → 104.
```

Of 2,121 postings with an existing tag, 28 fell outside the sector mapping and two had uncertain AI labels. That leaves 2,091 comparisons. Agreement measures consistency with the original tags. Sector gains count existing postings whose tags were repaired. The UK teaching count goes from 103 to 104 in the one-title example and reaches 105 in the full run.

## AI Use Disclosure

I used ChatGPT/Codex to generate the Python code and help write this README. I asked it to simplify the coding as much as possible, use the course cheat sheets, and speed up the default run and keep the chatbot working. I directed those revisions and reviewed every line to make sure I understood what it does. AI wrote the Python implementation and its revisions. Gemini assigned sectors to job titles and answers questions in live mode. It took many hours to make sure ChatGPT created the Python to my liking and making sure it only used formulas I understood.

Levi Michael Press · University of Georgia
