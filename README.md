# Pipeline Leak Detection from Vibration Data

A beginner machine-learning project: a binary classifier that looks at a few seconds of pipe vibration and predicts whether the pipe is **healthy** or **leaking**.

I'm a petroleum engineering student learning ML, and I'm doing this analysis myself as a learning exercise. It is not a production tool.

> **Status:** work in progress. Sections marked _TODO_ are not done yet.

---

## The data

This project uses a public dataset that **I did not create**:

> Theophilus Oluwaseyi Ariyo and Ajibola Matthew (2026). *Vibration-Based Pipeline Leak Detection Dataset Using Linear Discriminant Analysis (LDA)*. Mendeley Data, Version 1. Published 16 February 2026. https://doi.org/10.17632/575fy2tm7k.1

The dataset has vibration recordings from an MPU6050 accelerometer mounted on PVC pipe spans of 1.0 m, 1.5 m and 2.0 m. Each pipe was recorded in a healthy state and with a leak.

### Files from the original dataset

Only some of the original dataset is included here. All of these files are **unmodified** copies of the originals:

| File(s) | What it is | Used? |
|---|---|---|
| `data/raw/Healthy/span1.CSV`, `span2.CSV`, `span3.CSV` | Raw vibration recordings, healthy pipe | Yes (label: healthy) |
| `data/raw/Defective/Span1.CSV`, `Span2.CSV`, `Span3.CSV` | Raw vibration recordings, leaking pipe | Yes (label: leaking) |
| `data/raw/unused/Basedata.CSV` | Raw file not described in the authors' README | **No** (kept for completeness, but not used because it is undocumented) |
| `DATASET_README.txt` | The dataset authors' own README | Reference only |

Each CSV has the columns `Time(ms)`, `Ax(g)`, `Ay(g)`, `Az(g)`, `Accel_Magnitude(g)`. A file holds several recordings, and a new recording starts wherever the time column resets to near zero. The Healthy and Defective files contain **34 healthy** and **18 leaking** recordings in total.

The rest of the original dataset (the authors' extracted features, trained model and code) is **not** included. For those, go to the DOI link above.

### Data licence

The files listed above belong to the dataset's authors and are licensed under **[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)**. The authors' own README also describes the dataset as provided for research and academic use. If you reuse the data, please cite the authors as shown above.

---

## About this project

This is an **independent re-analysis**. It uses only the raw CSV files from the Healthy and Defective folders. It does **not** use or reproduce the dataset authors' code, extracted features or trained model. Everything here (data loading, windowing, features and model) is my own work.

This project is **not affiliated with or endorsed by** the dataset authors.

### Approach

1. Load the raw CSVs and label each recording by its folder (Healthy / Defective).
2. Explore the data and make plots.
3. Cut each recording into 3-second windows (about 43 readings each).
4. Compute about 10 simple features per window.
5. Train a classifier. All windows from one recording are kept together, either all in training or all in testing, so the model is never tested on a recording it has already seen.
6. Evaluate on held-out recordings.

Pipe length, sensor position, flow rate and recording length are deliberately **not** used as features. They describe the test setup, not the vibration, and a model could "cheat" by using them.

---

## Quick start

> _TODO: `requirements.txt` and `run_all.py` are not written yet._

```bash
git clone <repo-url>
cd leak-detection-ml
pip install -r requirements.txt
python run_all.py
```

Results and figures will be written to `reports/`.

### Predicting on a new file

> _TODO: `predict` script not written yet._

---

## Project layout

```
data/raw/           Original data, unmodified (Healthy/, Defective/, unused/)
data/processed/     Features I generate
notebooks/          Exploratory analysis
src/                My scripts
models/             Saved models
reports/            Results and figures
DATASET_README.txt  The dataset authors' original README, unmodified
```

---

## Results

> _TODO: no results yet. This section will be filled in after training and testing._

**Baseline to beat:** a "model" that always guesses *healthy* would be right about 65% of the time, because about 65% of the recordings (34 of 52) are healthy. A useful model has to do clearly better than that.

| Model | Accuracy | Precision (leak) | Recall (leak) | F1 (leak) |
|---|---|---|---|---|
| Always "healthy" baseline | ~65% | n/a | 0% | n/a |
| _TODO_ | _TODO_ | _TODO_ | _TODO_ | _TODO_ |

### Slow-flow-only check

> _TODO: results when testing only slow-flow healthy recordings against leaking ones (see Limitations)._

---

## Limitations

Please read these before trusting any result from this project.

- **Flow-rate bias.** Healthy pipes were recorded at two flow rates (45 L/min and 60 L/min), but leaking pipes were recorded **only at the slow rate (45 L/min)**. A model could learn "fast flow means healthy" instead of learning what a leak looks like. To check for this, I also test the model on slow-flow healthy recordings vs. leaking recordings only.
- **Very few leak cases.** There are only about **3 physical leak cases**. The 18 leaking recordings come from those few setups, so they are not independent examples. Good results here do **not** show that the method would work on other pipes, other leaks, or real pipelines.
- **Lab setup.** The data comes from short PVC pipe spans in a lab, using a low-cost accelerometer. Real pipelines (other materials, longer lengths, buried, with background noise) may behave very differently.
- **Small dataset overall.** 52 recordings is small for machine learning, so results may change a lot depending on which recordings end up in the test set.

---

## License

This repository contains two kinds of material under **different licences**:

| What | Owner | Licence |
|---|---|---|
| **Dataset files**: everything in `data/raw/`, plus `DATASET_README.txt` | Theophilus Oluwaseyi Ariyo and Ajibola Matthew | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The authors' README also describes the dataset as for research and academic use. See [The data](#the-data). |
| **Everything else** (code, notebooks, documentation, generated figures) | Me | _TODO: proposed MIT License (see `LICENSE`)_ |

The licence for my code does **not** apply to the dataset files. If you reuse the data, follow its licence and cite the original authors.

---

## Acknowledgements

Thanks to Theophilus Oluwaseyi Ariyo and Ajibola Matthew for collecting and publishing the dataset this project is built on.
