# gis-viz
Interactive maps of millions of features in Jupyter with [Lonboard](https://github.com/developmentseed/lonboard).

## Overview

A ~15 minute notebook on Lonboard, a Jupyter mapping widget from Development Seed. Instead of serializing data to GeoJSON like folium/ipyleaflet, Lonboard sends compressed GeoArrow/Parquet to the browser and renders it on the GPU with deck.gl, so it handles millions of features. The notebook covers:

- `viz()` one-liner on ~460k points
- GeoJSON vs. GeoParquet payload size, measured (~17x smaller)
- Data-driven styling and live layer updates
- GPU filtering with linked sliders (`DataFilterExtension`)
- Real tile polygons extruded in 3D (`PolygonLayer`)
- DuckDB SQL queries straight to a `HeatmapLayer`
- Bounding-box selection synced back to Python, and HTML export

## Dataset

Ookla Speedtest global mobile network performance, Q1 2019 (the dataset from Lonboard's own `internet-speeds` example):
- **Source**: [AWS Open Data Registry](https://registry.opendata.aws/speedtest-global-performance/)
- **Size**: ~3.2M tiles worldwide (~460k in the contiguous US)
- **Fields**: download/upload speed, latency, test & device counts per zoom-16 web mercator tile

State boundaries come from [Natural Earth](https://www.naturalearthdata.com/). The notebook downloads both datasets on the first run (~1 min, ~4 GB peak RAM) and caches the Ookla data in `data/`.

## Running

```
pip install -U "lonboard[geopandas]" "pyarrow>=16" duckdb palettable matplotlib
jupyter lab gis-viz.ipynb
```

On **I-GUIDE**, switch the notebook to the `geoai` conda environment first, then run the install cell and restart the kernel. Older preinstalled `pyarrow` versions cause `ValueError: Expected object with __arrow_c_array__ or __arrow_c_stream__ method`.

Lonboard maps are Jupyter widgets that need WebGL, so run the notebook in JupyterLab/Notebook or VS Code (not a static viewer such as GitHub's).
