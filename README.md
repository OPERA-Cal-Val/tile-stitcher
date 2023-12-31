# tile-stitcher

This tool provides a tool to create continuous rasters of global publicly available tiles such as Pekel Occurence and ESA 10 m land cover. This is a simpler cousin of [`dem-stitcher`](https://github.com/ACCESS-Cloud-Based-InSAR/dem-stitcher) without the need for basic post-processing (e.g. fractional pixel translation and vertical datum transformations).


The API can be summarized as
```
from tile_stitcher import get_raster_from_tiles

bounds = [-120.55, 34.85, -120.25, 35.15]
X, p = get_raster_from_tiles(bounds, tile_shortname='esa_world_cover_2021')

# X is an c x m x n numpy array, where c is the number of channels specified by `count`
# p is a dictionary (or a rasterio profile) including relevant GIS metadata; CRS is epsg:4326
```

The rasters are returned in the global lat/lon projection `epsg:4326` and the API assumes that bounds are supplied in this format.

```
import rasterio

with rasterio.open('esa_worlf_cover_2021_subset.tif', 'w', **p) as ds:
   ds.write(X)
```

# Installation

In order to easily manage dependencies, we recommend using dedicated project environments
via [Anaconda/Miniconda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html)
or [Python virtual environments](https://docs.python.org/3/tutorial/venv.html).

1. Clone the repository and navigate to it in the ternmal.
2. Install the environment with `mamba env update -f environment.yaml`
3. Activate the environment `conda activate tile-stitcher`
4. Install the library with `pip`

```
python -m pip install .
```

You can also install for development with `python -m pip install -e .`. Python 3.10+ is supported.

# Notebooks

We have notebooks to demonstrate common usage:

+ [Basic Demo](notebooks/Basic_Demo.ipynb)


# Datasets Supported 

The datasets supported are:

```
In [1]: from tile_stitcher.stitcher import DATASET_SHORTNAMES

In [2]: DATASET_SHORTNAMES
Out[2]: ['peckel_water_occ_2021',
 'esa_world_cover_2020',
 'esa_world_cover_2021',
 'hansen_annual_mosaic',
 's1_coherence_2020',
 'cop_100_lulc_discrete']
```
These correspond to
+ Pekel: https://global-surface-water.appspot.com/download
+ ESA World Cover (10 m) for 2020 and 2021: https://aws.amazon.com/marketplace/pp/prodview-7oorylcamixxc
+ Hansen annual mosaic: https://data.globalforestwatch.org/documents/941f17325a494ed78c4817f9bb20f33a/explore
+ S1 Coherence from December 2019 - Nov 2020: https://aws.amazon.com/marketplace/pp/prodview-iz6lnjbdlgcwa#resources
+ The copernicus 100 m LULC dataset from 2015 - 2019: https://land.copernicus.eu/global/content/annual-100m-global-land-cover-maps-available

See these [notebooks](notebooks/tile_creation) to see how these tiles are generated and organized.

# Dateline support

None curently.

# Contributing

We welcome contributions to this open-source package. To do so:

1. Create an GitHub issue ticket desrcribing what changes you need (e.g. issue-1)
2. Fork this repo
3. Make your modifications in your own fork
4. Make a pull-request (PR) in this repo with the code in your fork and tag the repo owner or a relevant contributor.

We use `flake8` and associated linting packages to ensure some basic code quality (see the `environment.yml`). These will be checked for each commit in a PR. Try to write tests wherever possible.

# Support

1. Create an GitHub issue ticket desrcribing what changes you would like to see or to report a bug.
2. We will work on solving this issue (hopefully with you).

# Acknowledgements

This tool was developed to support [OPERA](https://www.jpl.nasa.gov/go/opera).
