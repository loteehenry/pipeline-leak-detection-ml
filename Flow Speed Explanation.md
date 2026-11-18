# How I worked out which healthy recordings were slow and which were fast

## The short version

Inside each healthy CSV file, the recordings go **slow, fast, slow, fast, ...**

- Recordings **1, 3, 5, 7, 9, 11** are **slow** (45 L/min)
- Recordings **2, 4, 6, 8, 10, 12** are **fast** (60 L/min)

The count starts again at 1 in each file. All 18 leaking recordings are slow.

| Healthy file | Recordings | Slow | Fast |
|---|---|---|---|
| span1 (1.0 m pipe) | 12 | 6 | 6 |
| span2 (1.5 m pipe) | 10 | 5 | 5 |
| span3 (2.0 m pipe) | 12 | 6 | 6 |
| **Total** | **34** | **17** | **17** |

The rest of this note explains how I got there, so anyone can check it.

---

## Why I needed to know

Healthy pipes were tested at two water speeds, slow and fast. Leaking pipes were tested at slow speed only.

That creates a trap. Fast water probably makes a pipe shake more, so a program could learn "this shakes like fast water, so it must be healthy" without ever learning what a leak looks like. To check for this, I need to compare **slow healthy pipes against leaking pipes** (both slow), and for that I need to know which healthy recordings were slow.

The problem: **the CSV files don't say which recordings were slow and which were fast.** Each file just has time and shaking readings.

---

## Step 1: What the dataset README says, and why I didn't go with it

The dataset README says:

> Fast flow was only taken for the first and last positions of each span.

"Positions" are the spots along the pipe where the sensor was clamped. There were 6 spots per pipe, from one end to the other. For example, on the 1 m pipe they were at 0, 0.2, 0.4, 0.6, 0.8 and 1.0 m.

If the README were right, each pipe would have:

- 6 slow recordings (one at each spot), plus
- 2 fast recordings (only at the first and last spot)
- **= 8 recordings per pipe**

But when I counted the recordings in the healthy files, I got **12, 10 and 12**, not 8, 8 and 8. So the README's description doesn't match what's actually in the files.

---

## Step 2: What the authors' spreadsheet says

The authors also shared a spreadsheet called `healthy_features_windows_v2 .csv.xlsx`. I did **not** use any of their features or results. I only read the columns that describe how the experiment was run:

| Column | What it says |
|---|---|
| `point_id` | which sensor spot (P1 to P6) |
| `batch_index` | 0 = first run at that spot, 1 = second run |
| `flow` | `slow` or `fast` |
| `window_end_s` | where each 3-second slice of that run ends, in seconds |

The spreadsheet shows that at **every one of the 6 spots** on every pipe there were **two runs**:

- `batch_index 0` → `slow` (first run)
- `batch_index 1` → `fast` (second run)

So: 6 spots × 2 runs = **12 recordings per pipe**. That matches the 12 recordings I counted in span1 and span3. span2 has 10 because one spot, P1, is missing from the raw file.

---

## Step 3: Matching the spreadsheet to my CSV recordings

The spreadsheet tells me "at each spot, slow first and then fast," but it doesn't say "recording #3 in span1.CSV is slow." So I matched them up using **how long each recording lasted**.

It works like matching receipts to bank transactions by their amounts: if the amounts line up one after another, you know which receipt goes with which transaction.

- From **my CSV:** length of each recording = its last time reading ÷ 1000, in seconds.
- From **the spreadsheet:** the largest `window_end_s` for each run. That's where its last slice ends, so it should be **a little shorter** than the real recording, never longer.

### span1 (1.0 m)

| CSV recording | Length in my CSV (s) | Spreadsheet says | Last slice ends at (s) |
|---|---|---|---|
| 1 | 8.2 | P1, slow | 7.5 |
| 2 | 9.8 | P1, fast | 9.0 |
| 3 | 8.8 | P2, slow | 7.5 |
| 4 | 11.1 | P2, fast | 10.5 |
| 5 | 9.4 | P3, slow | 8.9 |
| 6 | 10.1 | P3, fast | 9.0 |
| 7 | 8.9 | P4, slow | 7.5 |
| 8 | 11.4 | P4, fast | 10.4 |
| 9 | 8.3 | P5, slow | 7.5 |
| 10 | 13.0 | P5, fast | 12.0 |
| 11 | 10.2 | P6, slow | 8.9 |
| 12 | 14.4 | P6, fast | 13.5 |

### span2 (1.5 m), with P1 missing from the raw file

| CSV recording | Length in my CSV (s) | Spreadsheet says | Last slice ends at (s) |
|---|---|---|---|
| 1 | 10.2 | P2, slow | 9.0 |
| 2 | 15.2 | P2, fast | 15.0 |
| 3 | 11.6 | P3, slow | 10.4 |
| 4 | 19.2 | P3, fast | 18.0 |
| 5 | 10.7 | P4, slow | 10.5 |
| 6 | 18.5 | P4, fast | 17.9 |
| 7 | 10.2 | P5, slow | 8.9 |
| 8 | 16.6 | P5, fast | 16.4 |
| 9 | 9.6 | P6, slow | 8.9 |
| 10 | 14.0 | P6, fast | 13.5 |

### span3 (2.0 m)

| CSV recording | Length in my CSV (s) | Spreadsheet says | Last slice ends at (s) |
|---|---|---|---|
| 1 | 12.4 | P1, slow | 11.9 |
| 2 | 13.8 | P1, fast | 13.5 |
| 3 | 9.4 | P2, slow | 8.9 |
| 4 | 18.3 | P2, fast | 18.0 |
| 5 | 11.3 | P3, slow | 10.5 |
| 6 | 15.0 | P3, fast | 13.5 |
| 7 | 9.4 | P4, slow | 9.0 |
| 8 | 23.4 | P4, fast | 22.5 |
| 9 | 8.8 | P5, slow | 7.5 |
| 10 | 18.0 | P5, fast | 16.5 |
| 11 | 9.5 | P6, slow | 9.0 |
| 12 | 15.5 | P6, fast | 15.0 |

### Why I trust this matching

1. **All 34 rows fit.** In every row, the spreadsheet number is just under the real recording length, which is what should happen.
2. **The opposite order doesn't fit.** If I assume fast comes first and slow second, it breaks straight away. For example, span3 recording 3 is only 9.4 s long, but the fast run at that spot has a slice ending at 18.0 s. A 9.4-second recording can't contain a slice that ends at 18 seconds.
3. **The fast run is usually the longer one in each pair,** which makes the pattern easy to see.

---

## What this means for my project

- **17 slow healthy recordings** and **17 fast healthy recordings**.
- **18 leaking recordings, all slow.** The authors' leaking spreadsheet lists only slow runs.
- My fair comparison is **17 slow healthy vs 18 leaking**, because then water speed can't be what tells them apart.

## Honest limits

- The dataset README says something different (fast only at the first and last spots). I went with the spreadsheet because the recording counts and lengths fit it and don't fit the README.
- The authors never wrote "recording #3 is slow." The odd/even rule comes from **my own matching** of recording lengths. The evidence is strong, but it's an inference, not a statement from the authors.
- The only way to be 100% sure would be to ask the dataset author (contact is in the dataset README).
- I used the authors' spreadsheet **only** to find out slow or fast. I did not use any of their features, model or code.

## How to check it yourself

```python
# Length of each recording in seconds, per file
lengths = (
    df.groupby(["file", "recording"])["Time(ms)"]
      .max()
      .div(1000)
      .round(1)
)
print(lengths)
```

Compare the printed lengths with the "Length in my CSV" column in the tables above. Then open the spreadsheet, filter by `sheet`, and compare with the largest `window_end_s` for each `batch_index`.
