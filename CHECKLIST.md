# Colab round trip: the tester's checklist

Test content only. This checklist covers the Colab leg of the smoke test for 5PAM2024
*Statistical Modelling*: the round trip from GitHub through Colab to download, the 14
behaviours that Google's documentation leaves open, as listed in the module team's research
notes on Colab, and one more: the submission mode (observation 15). Observation 12 is a cohort
test for week 1 or a rehearsal, not part of a single tester's session.

**Rules for the record.** Write down what you see, word for word where a label or message
matters. If something did not happen or was not checked, write "not observed" or "not checked":
never fill a gap from memory or from documentation. Put nothing personal into the notebook or
into any AI prompt. Use a separate record sheet (section D) for each session, device or account.

## A. Before you start

1. Use an ordinary **personal** Google account, aged 18 or over, on Colab's **free** tier.
2. Note the date and time, the browser and its version (for example Chrome: menu > Help > About
   Google Chrome), the device (a managed lab PC or your own laptop) and the network.
3. On the same day, ask the module team to record Google's published package list for Colab
   (`googlecolab/backend-info` on GitHub), so that the versions the notebook prints can be set
   against it (step B2).

## B. The round trip

| Step | Do this | Expected (by design) | Record |
|---|---|---|---|
| B1 | Open the **Open in Colab** link in `README.md` | The notebook opens in Colab, read from GitHub, with no sign-in to GitHub | Did it open? Any warning banner? Seconds until a runtime connected |
| B2 | Run the first cell (VERSIONS) | It prints the run time, Python, platform, `In Colab True` and the package versions; any version that differs from the tested one is followed by "(tested with ...)" | Copy the whole output into the record sheet (observation 8) |
| B3 | Run SET-UP, then SEED | "Set-up complete." and no error | Any warning text |
| B4 | Run the cell that loads `shared` | "Loaded SHARED ..." with `source  https://raw.githubusercontent.com/...`; no "Could not download" line | The source line; any HTTP error (note any 429) |
| B5 | Run the model, influence and plot cells | Tables of coefficients and ANOVA Types I, II and III; a table of the five largest Cook's distances; two plots | Any warning text (pink or grey output boxes) |
| B6 | Run the exercise E1 cell unchanged | It stops with **one line**: `ExerciseNotDone: Exercise not finished: replace the ... in ...` | Was it one line, or a long traceback? |
| B7 | Run the stretch cell unchanged | "Stretch S1 not attempted: skipped." | |
| B8 | **Runtime > Run all**, with E1 still unfinished | Execution stops at the E1 cell with the same one-line message | Where did it stop? |
| B9 | Complete E1 as its instruction says, and run it | "E1 recorded: PE ~ AT, n = 200" | |
| B10 | Run the first assessed cell (below DATASET ENTRY) before entering an ID | `DatasetError: No valid dataset is loaded. Run the DATASET ENTRY cell ...` | |
| B11 | Type `T99` between the quotes in the DATASET ENTRY cell and run it | `DatasetError: 'T99' is not a dataset ID for this lab ...` | |
| B12 | Type `T07` and run the cell | "Loaded T07: smoke-test individual data T07 ...", `120 rows x 8 columns, 5893 bytes`, `sha256  df8c1fd9e4dc...46ce03f79` | Copy the four lines |
| B13 | Run both assessed cells | Each starts "Using dataset T07 (sha256 df8c1fd9e4dc...)"; an ANOVA table, three Cook's distances, one plot titled "Dataset T07" | |
| B14 | **Runtime > Restart session and run all** (exact label?) | Every cell runs, top to bottom, with no error | Exact menu label; any error |
| B15 | Before saving a copy, reload the browser tab | | Did the edits (E1, `T07`) survive the reload? (observation 10) |
| B16 | Save a copy in Drive (record the exact menu path and label) | A copy opens, titled "Copy of prototype_lab.ipynb" or similar | Exact label; the copy's title (observation 10) |
| B17 | In the copy, **Restart session and run all**, then download the notebook as `.ipynb` (record the exact menu path) | A file `prototype_lab.ipynb` (or similar) downloads | Exact menu labels; the file name |
| B18 | Open the downloaded file in a text editor and search for `Loaded T07` and `Using dataset T07` | Both found: the outputs and the dataset ID are saved in the file | Found or not (observation 10) |
| B19 | In the downloaded file, search for `"colab"` and `"metadata"` | | Copy any `colab` metadata block that appears |

## C. The observations

Observations 1 to 14 follow the research notes; observation 15 was added after the module's
Gate 2 review. Observations 3, 6 and 7 have a second stage that needs a further file on this test
branch: send the downloaded file to the module team, who add it to the branch only after that
push is approved. **Observation 12 is not observed before Gate 2**: it needs the cohort.

| No. | Observe | How | Record |
|---|---|---|---|
| 1 | Spark icon, chat, inline completions and the Data Science Agent | Look in the notebook toolbar, the side panel and while typing in a code cell; note any consent prompt on first use. Repeat on a lab PC and on a laptop | Present or absent, for each feature and device; the consent prompt's wording |
| 2 | The exact labels under **Settings > AI Assistance** and **Edit > Notebook settings** | Open both and copy every label and its default state | The labels, word for word |
| 3 | Whether "Hide generative AI features" writes `colab.generative_ai_disabled` into the downloaded `.ipynb`; whether it survives GitHub > Open in Colab > Save a copy; whether one click reverses it | Stage 1: in a saved copy, set the option, download, search the file for `generative_ai_disabled`; try to reverse it. Stage 2: the module team add that file to the branch; open it with Open in Colab, Save a copy, and check the setting | The key and value found; clicks needed to reverse; the state after stage 2 |
| 4 | Whether `google.colab.ai` works on a free account with the interface hidden | With the AI features hidden (observation 3), add a cell: `from google.colab import ai` and `print(ai.generate_text("Reply with the word OK."))`, and run it | Output or error message |
| 5 | Whether AI-generated or agent-inserted cells leave any trace in the `.ipynb` | In a saved copy, let the AI insert or generate one cell (any harmless request); download; search for new metadata keys on that cell | Keys found, or "none" |
| 6 | Whether Custom Instructions or Learn Mode, set in the source notebook, survive the round trip | Stage 1: set them in a saved copy, download, search the file. Stage 2: the module team add the file to the branch; open, Save a copy, check | Keys found; state after stage 2 |
| 7 | Whether a runtime pin in the source notebook is inherited from GitHub, and the connection time | Stage 1: **Runtime > Change runtime type > Runtime version**, choose a past version; note the connection time; download and search the file. Stage 2: the module team add the file; open it with Open in Colab and see which version connects | Labels; seconds to connect; key found; version after stage 2 (run the VERSIONS cell) |
| 8 | Versions printed by the first cell, against `backend-info` that day | Step B2, and the published list recorded in step A3 | Each package: printed version and published version |
| 9 | Idle behaviour at 60, 90 and 120 minutes; one full 3-hour session of normal use | Idle: after step B14, leave the tab alone. At 60, 90 and 120 minutes look at it **without clicking** (is there a "disconnected" or "reconnect" message?). At the end, add a cell `print(SEED)`: a `NameError` means the variables were lost. Separately, one 3-hour session of ordinary work | Minutes until disconnection, if any; what was lost (variables, outputs, the notebook itself); the 3-hour session's interruptions |
| 10 | Unsaved GitHub view after a reload; the exact labels of Save a copy in Drive and Download .ipynb; outputs in the downloaded file | Steps B15 to B18 | As in those steps |
| 11 | `#copy=true` on the Open in Colab link | Open the `#copy=true` link in `README.md` | Does a copy dialog appear? Its wording |
| 12 | Thirteen near-simultaneous opens and raw-URL loads; any HTTP 429. **Cohort test, in the week-1 practice run or a rehearsal; not a pre-Gate-2 observation** | Only with the cohort: everyone opens the link and runs B2 to B4 at the same moment | How many loaded from the URL; any "Could not download" line and its reason (429?) |
| 13 | Cookie or `googleusercontent.com` blocking on the managed lab browser; whether Edge works | On a lab PC, open the link in each installed browser, Edge included; run B2 to B4 | Per browser: opens, connects, runs; any cookie or blocked-content warning |
| 14 | Whether a Herts account can sign in to Colab (fact-finding only: the module uses personal accounts) | Try to open the link signed in with a university account | Allowed or refused; the message shown |
| 15 | The submission run: what "Restart and run all" does with an unfinished exercise (this informs the module's `SUBMITTING` flag, which will turn unfinished exercises into notices so that the run completes) | In a saved copy, leave E1 unfinished and type `T07` in the DATASET ENTRY cell. (a) Use the menu item that restarts the session and runs all cells. (b) Note where execution stops. (c) Download the notebook as `.ipynb`; open it in a text editor and search for `Loaded T07` and `Using dataset T07` | The exact menu path and label for (a), word for word, and any keyboard shortcut shown; the cell where execution stopped and the message shown; whether the downloaded file holds the outputs of the cells that ran (both strings found or not) |

## D. Record sheet (copy once per session)

| Field | Entry |
|---|---|
| Date and time (UK) | |
| Tester | |
| Device (managed lab PC or own laptop) and operating system | |
| Browser and version | |
| Network (campus wired, campus Wi-Fi, home) | |
| Google account type (personal or Herts) | |
| Account age band (18 or over, or under 18; never a date of birth) | |
| Colab tier (free or paid) | |
| Commit or date of `backend-info` compared with (step A3) | |

| Step or observation | Result (pass, fail, or what was seen) | Notes, exact wording, timings |
|---|---|---|
| B1 | | |
| B2 / 8 | | |
| B3 | | |
| B4 | | |
| B5 | | |
| B6 | | |
| B7 | | |
| B8 | | |
| B9 | | |
| B10 | | |
| B11 | | |
| B12 | | |
| B13 | | |
| B14 | | |
| B15 / 10 | | |
| B16 / 10 | | |
| B17 / 10 | | |
| B18 / 10 | | |
| B19 | | |
| 1 | | |
| 2 | | |
| 3 (stage 1) | | |
| 3 (stage 2) | | |
| 4 | | |
| 5 | | |
| 6 (stage 1) | | |
| 6 (stage 2) | | |
| 7 (stage 1) | | |
| 7 (stage 2) | | |
| 9 (idle 60 / 90 / 120 min) | | |
| 9 (3-hour session) | | |
| 11 | | |
| 12 (cohort test, week 1 or rehearsal) | | |
| 13 | | |
| 14 | | |
| 15 (menu path; outputs kept?) | | |
