# Project: Pipeline Leak Detection from Vibration Data (ML)

## What this is
A learning project. Goal: build my own binary classifier that looks at a few seconds of pipe vibration and says **healthy** or **leaking**. I'm a petroleum engineering student and a complete ML beginner, so I'm doing the analysis myself to learn. Your job is the routine setup work (README, .gitignore, licence, requirements, folder setup, git) and explaining things when I ask. **Don't write the analysis or modelling for me unless I ask.**

## Data
- Source: Theophilus Oluwaseyi Ariyo and Ajibola Matthew (2026), "Vibration-Based Pipeline Leak Detection Dataset Using Linear Discriminant Analysis (LDA)", Mendeley Data, Version 1, published 16 February 2026. https://doi.org/10.17632/575fy2tm7k.1
- Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). The authors' README also describes it as for research and academic use. Credit the authors in the README. This project is an independent re-analysis, not affiliated with or endorsed by them.
- Sensor: MPU6050 accelerometer on PVC pipe spans of 1.0, 1.5 and 2.0 m. Columns: `Time(ms)`, `Ax(g)`, `Ay(g)`, `Az(g)`, `Accel_Magnitude(g)`. Time is in milliseconds.
- Files I use (in `data/raw/`):
  - `Healthy/span1.CSV`, `span2.CSV`, `span3.CSV`
  - `Defective/Span1.CSV`, `Span2.CSV`, `Span3.CSV` (capital S)
- Also kept in the repo, unmodified, but not used for analysis:
  - `data/raw/unused/Basedata.CSV`
  - `DATASET_README.txt` (the authors' original README)
- Only these files from the original dataset are in the repo. All are unmodified.
- The folder name is the label. Each file holds several recordings. A new recording starts where the time column resets to near 0. Total: 34 healthy and 18 leaking recordings.

## Hard rules
- Do **not** use `Basedata.CSV`. It's undocumented.
- Do **not** use the dataset authors' code, extracted features or trained model. Everything is built from the raw CSVs only.
- When splitting data, keep all windows from one recording together (either all train or all test).
- Never use pipe length, sensor position, flow rate or recording length as features.

## Known caveats (mention in the README)
- Healthy pipes were recorded at two flow rates (45 and 60 L/min), leaking pipes only at the slow rate, so a model might learn "fast flow = healthy". Check by also testing slow-healthy vs leaking only.
- There are only about 3 physical leak cases, so results don't prove it works on real pipelines.
- Baseline to beat: always guessing "healthy" is right about 65% of the time.

## Planned workflow
1. Load the data and label it by folder.
2. EDA and plots.
3. Cut recordings into 3-second windows (about 43 readings each).
4. Compute about 10 simple features per window.
5. Train, keeping recordings grouped.
6. Test on held-out recordings.
7. Package it: a saved model, a `predict` script (give it a CSV and it says healthy or leaking), and the README.

## Intended repo layout
```
data/raw/        Healthy/, Defective/, unused/ (Basedata.CSV)
data/processed/  features I generate
notebooks/       EDA
src/             my scripts
models/          saved models
reports/         results and figures
README.md, DATASET_README.txt, requirements.txt, run_all.py, .gitignore, LICENSE
```
Goal for visitors: clone the repo, run `pip install -r requirements.txt`, run `python run_all.py`, then see results in `reports/`.

## Environment
Windows, VS Code, Python 3. The raw data is small (under 400 KB), so commit it to git.

## How to work with me
Use plain language without jargon. Briefly explain what you did and why, so I learn the tooling too.