# StatMod smoke-test fixture (test content only)

This branch exists only to test the lab round trip of the module 5PAM2024 *Statistical
Modelling* (University of Hertfordshire): GitHub, then Google Colab, then download. It holds no
teaching material and no answers, and it will be deleted once the test is recorded.

## Contents

| Path | What |
|---|---|
| `prototype_lab.ipynb` | The prototype lab notebook, with outputs and metadata cleared. It opens in practice mode (`SUBMITTING = False`); `CHECKLIST.md` observation 15 tests the submission run |
| `data/smoke_shared.csv` | 200 rows: the shared dataset that the formative steps load automatically |
| `data/smoke_T07.csv` | 120 rows: the individual dataset behind the test ID `T07` |
| `CHECKLIST.md` | The tester's step-by-step checklist and record sheet |

## Open in Colab

Colab opens notebooks from GitHub with the link pattern

```text
https://colab.research.google.com/github/uh-pam/statmod/blob/<branch>/<path>.ipynb
```

For this branch (`smoke-test`):

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/uh-pam/statmod/blob/smoke-test/prototype_lab.ipynb)

```text
https://colab.research.google.com/github/uh-pam/statmod/blob/smoke-test/prototype_lab.ipynb
```

The same link with `#copy=true` at the end is meant to bring up Colab's copy dialog on opening
(checklist, observation 11):

```text
https://colab.research.google.com/github/uh-pam/statmod/blob/smoke-test/prototype_lab.ipynb#copy=true
```

## Data from raw URLs

The notebook loads its data from raw GitHub URLs, so nobody uploads files:

```text
https://raw.githubusercontent.com/uh-pam/statmod/<branch>/<path>
https://raw.githubusercontent.com/uh-pam/statmod/smoke-test/data/smoke_shared.csv
https://raw.githubusercontent.com/uh-pam/statmod/smoke-test/data/smoke_T07.csv
```

Each file's sha256 is written in the notebook's set-up cell and checked on loading. If the
download fails, the notebook says so and uses `data/<file>` beside the notebook instead (the
offline fallback for lab PCs).

| File | Rows | Bytes | sha256 |
|---|---|---|---|
| `data/smoke_shared.csv` | 200 | 9,772 | `c4617c32b247d19444826d9ae84b9c657b97ded172e8286d57d49748cadbc93d` |
| `data/smoke_T07.csv` | 120 | 5,893 | `df8c1fd9e4dc0f22711aa532cce44c93afbf79bc79ad81467dfae1e46ce03f79` |

## Data source and licence

Both files are subsamples of the *Combined Cycle Power Plant* dataset: Tüfekci, P. and Kaya, H.
(2014), UCI Machine Learning Repository, <https://doi.org/10.24432/C5002N>, licensed under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

Changes made: exact duplicate rows removed; 320 distinct rows drawn without replacement with a
fixed seed and split into two disjoint files; three columns added:

- `ccpp_row`: the row's number in the UCI file `data.csv` (1 = first data row);
- `temp_band`: from `AT`: `cool` below 15 °C, `mild` from 15 to below 25 °C, `warm` from 25 °C;
- `humidity_band`: from `RH`: `lower` below 75 %, `higher` from 75 %.

The original columns are `AT` ambient temperature (°C), `V` exhaust vacuum (cm Hg), `AP` ambient
pressure (mbar), `RH` relative humidity (%) and `PE` net hourly electrical energy output (MW).
The two bands exist only to test formulas with categorical terms.
