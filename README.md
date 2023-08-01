# MoisesDB
Moises Dataset for Source Separation

# Download the data

Please download the dataset at our research [website](https://developer.moises.ai/research), extract it and configure the environment variable `MOISESDB_PATH` accordingly.

```
export MOISESDB_PATH=./moises-db-data
```

The directory structure should be

```
moisesdb:
    moisesdb_v0.1
        track uuid 0
        track uuid 1
        .
        .
        .
```

# Install

You can install this package with

```
pip install moisesdb
```

# Usage

## `MoisesDB`

After downloading and configuring the path for the dataset, you can create an instance of `MoisesDB` to access the tracks. You can also provide the dataset path with the `data_path` argument.

```
from moisesdb.dataset import MoisesDB

db = MoisesDB(
    data_path='./moisesdb',
    sample_rate=44100
)
```

The `MoisesDB` object has iterator properties that you can use to access all files within the dataset.

```
n_songs = len(db)
track = db[0]  # Returns a MoisesDBTrack object
```

## `MoisesDBTrack`

The `MoisesDBTrack` object holds information about a track in the dataset, perform on-the-fly mixing for stems and multiple sources within a stem.

You can access all the stems and mixture from the `stem` and `audio` properties. The `stem` property returns a dictionary whith available stems as keys and `nd.array` on values. The `audio` property results in a `nd.array` with the mixture.

```
track = db[0]
stems = track.stems  # stems = {'vocals': ..., 'bass': ..., ...}
mixture track.audio # mixture = nd.array
```

The `MoisesDBTrack` object also contains other non-audio information from the track such as:
- `track.id`
- `track.provider`
- `track.artist`
- `track.name`
- `track.genre`
- `track.sources`
- `track.bleedings`
- `track.activity`

The stems and mixture are computed on-the-fly. You can create a stems-only version of the dataset using the `save_stems` method of the `MoisesDBTrack`.

```
track = db[0]
path =  './moises-db-stems/0'
track.save_stems(path)
```

# Performance Evaluation

We run a few source separation algorithms as well as oracle methods to evaluate the performance of each track of the `MoisesDB`. These results are located in `csv` files at the `benchmark` folder.

# Citing

If you used the `MoisesDB` dataset on your research, please cite the following paper.

```
@misc{pereira2023moisesdb,
      title={Moisesdb: A dataset for source separation beyond 4-stems}, 
      author={Igor Pereira and Felipe Araújo and Filip Korzeniowski and Richard Vogl},
      year={2023},
      eprint={2307.15913},
      archivePrefix={arXiv},
      primaryClass={cs.SD}
}
```

