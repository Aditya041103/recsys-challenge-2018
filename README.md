# recsys-challenge-2018

Solution for the 2018 Spotify RecSys Challenge by the team **Definitive Turtles**

## Table of Contents
- [Overview](#overview)
- [Requirements](#requirements)
- [Installation](#installation)
- [Data Setup](#data-setup)
- [How to Run](#how-to-run)
- [Script Descriptions](#script-descriptions)
- [Configuration](#configuration)
- [Output](#output)
- [Reference Environment](#reference-environment)

## Overview

This project implements a recommendation system solution for the Spotify RecSys Challenge 2018, which involves automatic playlist continuation. The solution uses collaborative filtering and name-based matching techniques to recommend songs for incomplete playlists.

## Requirements

### System Requirements
- **RAM**: 16GB minimum
- **Disk Space**: ~40GB free space
- **Python**: 3.5+

### Python Packages
Install the required packages using:
```bash
pip install -r requirements.txt
```

Or install them manually:
```bash
pip install pandas==0.22.0 numpy==1.14.0 matplotlib==2.0.2 scipy==1.0.0
```

## Installation

1. Clone this repository:
```bash
git clone <repository-url>
cd recsys-challenge-2018
```

2. Install the required dependencies:
```bash
pip install -r requirements.txt
```

3. Create the necessary directories for raw data:
```bash
mkdir -p data_raw/million_playlist_dataset
mkdir -p data_raw/challenge_set
```

## Data Setup

1. **Download the Million Playlist Dataset** from the [Spotify RecSys Challenge](https://www.aicrowd.com/challenges/spotify-million-playlist-dataset-challenge) (requires registration)

2. **Place the data files**:
   - Place all Million Playlist Dataset JSON files (`mpd.slice.*.json`) into `data_raw/million_playlist_dataset/`
   - Place the challenge set JSON file(s) into `data_raw/challenge_set/`

Your directory structure should look like:
```
recsys-challenge-2018/
├── data_raw/
│   ├── million_playlist_dataset/
│   │   ├── mpd.slice.0-999.json
│   │   ├── mpd.slice.1000-1999.json
│   │   └── ...
│   └── challenge_set/
│       └── challenge_set.json
├── data/
│   ├── million_playlist_dataset/
│   └── challenge_set/
├── output/
└── ...
```

## How to Run

### Option 1: Run all scripts automatically
```bash
./run_all.sh
```

### Option 2: Run scripts step by step

**Step 1**: Convert Million Playlist Dataset to CSV
```bash
python 1_mpd_tocsv.py
```

**Step 2**: Convert Challenge Set to CSV
```bash
python 2_cs_tocsv.py
```

**Step 3**: Assign tasks to challenge playlists
```bash
python 3_tasks.py
```

**Step 4**: Generate recommendations for title-only playlists (0 tracks)
```bash
python 4_0t.py
```

**Step 5**: Generate recommendations for 1-track playlists
```bash
python 5_1t.py
```

**Step 6**: Generate recommendations for 100-track first playlists
```bash
python 6_100f.py
```

**Step 7**: Generate recommendations for remaining tasks (run with parameters 0-4)
```bash
for i in $(seq 0 4); do python 7_rest.py $i; done
```

**Step 8**: Merge all recommendation outputs
```bash
python 8_merge.py
```

**Step 9**: Format and fix the final submission
```bash
python 9_format_and_fix.py output/merged.csv output/submission.csv
```

## Script Descriptions

| Script | Description |
|--------|-------------|
| `1_mpd_tocsv.py` | Converts the Million Playlist Dataset JSON files to CSV format, creating playlist metadata, song metadata, and playlist-song mappings |
| `2_cs_tocsv.py` | Converts the Challenge Set JSON to CSV format |
| `3_tasks.py` | Assigns challenge types to playlists (0t, 1t, 5t, 10t, 25f, 25r, 100f, 100r) |
| `4_0t.py` | Generates recommendations for playlists with only a title (0 tracks) using name-based matching |
| `5_1t.py` | Generates recommendations for playlists with 1 track using KNN collaborative filtering |
| `6_100f.py` | Generates recommendations for 100-track "first" playlists |
| `7_rest.py` | Handles remaining task types (5t/5nt, 10t/10nt, 25f, 25r, 100r) - run with parameter 0-4 |
| `8_merge.py` | Merges all individual recommendation files into one |
| `9_format_and_fix.py` | Formats the submission, removes duplicates/invalid entries, and fills missing recommendations |

## Configuration

### Thread Configuration
Scripts `5_1t.py`, `6_100f.py`, and `7_rest.py` contain a `threads` variable that controls parallel processing:

```python
threads = 4  # Adjust based on your CPU cores
```

Increase this value to speed up processing if you have more CPU cores available.

## Output

After running all scripts, you will find:
- `output/merged.csv` - Combined recommendations from all tasks
- `output/submission.csv` - Final formatted submission file
- `output/submission.csv.gz` - Compressed submission ready for upload

## Reference Environment

We ran the models using the following Python version and packages:
```
Python 3.5.2 (we used the conda environment)
pandas 0.22.0
numpy 1.14.0
matplotlib 2.0.2
scipy 1.0.0
```

## License

See the [LICENSE](LICENSE) file for details.
