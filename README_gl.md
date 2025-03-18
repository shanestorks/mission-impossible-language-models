# Setup

## First Run
Follow the below steps for initial setup:

1. Clone this fork:

```
cd path/to/where/you/want/to/clone/the/repo
git clone https://github.com/shanestorks/mission-impossible-language-models.git
cd mission-impossible-language-models
```

2. Load the same modules we used before, create a new environment `milm`, and install the dependencies from the repo:

```
module load python3.11-anaconda/2024.02
module load cuda/11.3.0
conda create -n milm python=3.9
source activate milm
pip install -r requirements.txt
```

3. Download BabyLM data:
```
cd data
wget https://github.com/babylm/babylm.github.io/raw/main/babylm_data.zip
unzip babylm_data.zip
```

*Note: you can also just use Shane's downloaded and tagged data at `/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data`, enabling you to skip Steps 3-4.*

4. Tag the BabyLM data.
```
export BABYLM_DATA_PATH="full/path/to/babylm_data"
find "${BABYLM_DATA_PATH}/babylm_10M" -type f -name "*.train" | xargs python3 data/tag.py
find "${BABYLM_DATA_PATH}/babylm_100M" -type f -name "*.train" | xargs python3 data/tag.py
find "${BABYLM_DATA_PATH}/babylm_dev" -type f -name "*.dev" | xargs python3 data/tag.py
find "${BABYLM_DATA_PATH}/babylm_test" -type f -name "*.test" | xargs python3 data/tag.py
```

*Note: This requires GPU and takes several minutes. I recommend creating a Slurm job to run it. Also, some files will fail due to there being texts longer than the tagging model's sequence length.*

5. To be continued... (currently waiting for data to tag)

## Later Runs

Activate the environment:

```
module load python3.11-anaconda/2024.02
module load cuda/11.3.0
source activate milm
```

## Notes on BabyLM Data

BabyLM is downloaded in Shane's Turbo volume at `/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data`. All the data files for BabyLM can be listed with the following commands:

```
find /nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data -type f -name "*.train"
find /nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data -type f -name "*.dev"
find /nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data -type f -name "*.test"
```

They return the following lists of files:

```
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_100M/simple_wiki.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_100M/bnc_spoken.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_100M/childes.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_100M/open_subtitles.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_100M/gutenberg.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_100M/switchboard.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_10M/simple_wiki.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_10M/bnc_spoken.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_10M/childes.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_10M/open_subtitles.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_10M/gutenberg.train \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/train_10M/switchboard.train
```

```
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/simple_wiki.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/childes.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/switchboard.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/open_subtitles.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/gutenberg.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/bnc_spoken.dev
```

```
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/test/childes.test \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/test/switchboard.test \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/test/bnc_spoken.test \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/test/gutenberg.test \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/test/simple_wiki.test \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/test/open_subtitles.test
```

The above lists can be used as command-line arguments for `data/tag.py`, which tags BabyLM data for generating impossible languages. For example:

```
python3 data/tag.py /nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/simple_wiki.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/childes.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/switchboard.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/open_subtitles.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/gutenberg.dev \
/nfs/turbo/coe-sstorks/sstorks/mission-impossible-language-models/data/babylm_data/dev/bnc_spoken.dev
```