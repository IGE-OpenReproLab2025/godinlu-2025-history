# history

**Historical Images for Surface Topography Reconstruction over the Last 50 Years**

## Scientific context

In the 20th century, the Earth's surface changed in a significant way, caused by global warming and anthropic pressure. The melting of glacier, changes of floors and urbanisation can be some indicators of theses changes. So it's important to know the evolution of the surface since the paste 30 years.

With the modern satelites we can produce DEM (Digital Elevation Model) and with difference of DEMs we can estimate the vertical evolution of surface such as glacier and much more (Berthier et al., 2023; Lacroix, 2016). But the majority of this data existe only since 2000 (ASTER). But before numerical image a lot of images of earth are been acquired on photographic tape by satelites or planes. A lot of these data comes from spy mission and are for the majority declassified and free to access. All of these data have a huge potential to follow the multi-decade earth changes.

|Name|Cover|Period|Resolution|Source|
|--|--|--|--|--|
|Corona (KH-4)|Global|1959-1972|2-8 m|US Geological Survey|
|Hexagon(KH9-pano)|Global|1971-1984|< 1 m|US Geological Survey|
|Hexagon (KH9-carto)|Global|1973-1980|6-9 m|US Geological Survey|
|Resurs-F1/2|Global|1974-1999|5-8 m|Sojuzkarta|
|Aerial|Regional|1930-|< 1 m|Local geographical institutes|

***Table 1** : Characteristics of the historical data of this project (non-exhaustive list)*

However, these images are challenging to process. Various processing pipelines have been developed, but their performance has rarely been thoroughly evaluated. The ongoing international HISTORY project (Historical Images for Surface TOpography Reconstruction over the Last 50 Years), led by Amaury Dehecq and involving multiple collaborators, aims to compare these pipelines using a shared dataset of preprocessed historical images. The goal is to assess their performance against reference DEMs and eventually merge the best tools from each to create a complete, optimized processing pipeline.

|Pipeline|Software|A/M|Data|Institutes|Reference|
|---|---|---|---|---|---|
|SpyMicMac|MicMac, Python|A|KH9-carto|U. of Ulster, UK|Non publication|
|ASPy|ASP, Python|A|KH9-carto|IGE, France|(Dehecq et al., 2020)|
|HSfM|ASP, Metashape, Python|A|Aéro|U. of Washington, USA|(Knuth et al., 2023)|

***Table 2**: List of existing processing pipelines for historical data to be evaluated in the HISTORY project(A : Automatic, M : Manual).*

## Internship objectives

My objective during this internship is to contribute to the HISTORY project by providing preprocessed data. To do this, I have developed a Python package called [hipp](https://github.com/shippp/hipp) (Historical Image Pre-processing), which integrates components from existing pipelines to preprocess historical images. In parallel, I have also created the [history](https://github.com/shippp/history) repository, which contains all the notebooks that use hipp to generate the dataset for the HISTORY project. These notebooks are fully reproducible but require USGS credentials and significant computing time.

## Dataset for the HISTORY project

| Dataset Name | Date | Images Count | Preprocess Notebook | Images Provider| Preprocessed images size | Raw images size| Bounding Box |
|--|--|--|--|--|--|--|--|
| Casa Grande Aerial | 1978/09/06 | 37 | [casa_grande_aerial.ipynb](notebooks/casa_grande_aerial.ipynb)| [USGS EE](https://earthexplorer.usgs.gov/), [single frame dataset](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-aerial-photography-aerial-photo-single-frames?qt-science_center_objects=0#qt-science_center_objects)| 2.6 Go | 7.2 Go |-111.913862   32.699015 -111.685857   32.942928|
| Casa Grande KH-9 PC | 1978/03/25 | 6 |[casa_grande_kh9pc.ipynb](notebooks/casa_grande_kh9pc.ipynb) | [USGS EE](https://earthexplorer.usgs.gov/), [Declass 3](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-declassified-data-declassified-satellite-imagery-3) | 43 Go | 47 Go |-113.535   32.28  -109.812   33.25|
| Casa Grande KH-9 MC | 1978/03/25 | 4 | [casa_grande_kh9mc.ipynb](https://github.com/shippp/history/blob/main/notebooks/casa_grande_kh9mc.ipynb)| [USGS EE](https://earthexplorer.usgs.gov/), [Declass 2](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-declassified-data-declassified-satellite-imagery-2?qt-science_center_objects=0#qt-science_center_objects) | 8.2 Go | 8.9 Go |-112.645   30.782 -110.574   35.053|
| Iceland Aerial | 1980/08/22 | 125 | [iceland_aerial.ipynb](notebooks/iceland_aerial.ipynb)| FTP | 14 Go | 12 Go |-19.74345739  63.42948958 -17.68651768  63.86200756|
| Iceland KH-9 PC | 1980/08/22 | 6 | [iceland_kh9pc.ipynb](notebooks/iceland_kh9pc.ipynb) | [USGS EE](https://earthexplorer.usgs.gov/), [Declass 3](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-declassified-data-declassified-satellite-imagery-3) | 33 Go | 30 Go |-23.362  62.923 -15.674  64.405|
| Iceland KH-9 MC | 1980/08/22 | 4 |[iceland_kh9mc.ipynb](https://github.com/shippp/history/blob/main/notebooks/iceland_kh9mc.ipynb) | [USGS EE](https://earthexplorer.usgs.gov/), [Declass 2](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-declassified-data-declassified-satellite-imagery-2?qt-science_center_objects=0#qt-science_center_objects) | 8.1 Go | 6.4 Go |-21.946  61.43  -16.633  66.089|

## Images Preprocessing

here is a short explanation of each preprocessing methods:

### Aerial Images

The preprocessing method mainly following [Knuth et al. (2023)](https://www.sciencedirect.com/science/article/pii/S0034425722004850?via%3Dihub):

1. If RGB convert images to grayscale by using the luminance formula.
2. Detect fiducials markers at subpixel level by performing template matching
3. Outliers are rejected by removing markers that have a too low matching score.
4. Compute affine transformations to align the detected fiducials markers with calibrated fiducials markers.
5. Apply the transformation and crop the images around the center of fiducials markers and apply CLAHE.

Data preprocessing notebooks are available here:

- [casa_grande_aerial.ipynb](https://github.com/shippp/history/blob/main/notebooks/casa_grande_aerial.ipynb)
- [iceland_aerial.ipynb](https://github.com/shippp/history/blob/main/notebooks/iceland_aerial.ipynb)

### KH-9 Panoramic Camera Images

The preprocessing method mainly following [Ghuffar et al. (2023)](https://tc.copernicus.org/articles/17/1299/2023/):

1. The image pieces are joined into a single composite image using ASP’s [`image_mosaic`](https://stereopipeline.readthedocs.io/en/latest/tools/image_mosaic.html) command. It uses IP matching and affine transformation to align the pieces together.
2. The film corners are selected manually.
3. The image is rotated and cropped so that the film edge is horizontal.

Data preprocessing notebooks are available here:

- [casa_grande_kh9pc.ipynb](https://github.com/shippp/history/blob/main/notebooks/casa_grande_kh9pc.ipynb)
- [iceland_kh9pc.ipynb](https://github.com/shippp/history/blob/main/notebooks/iceland_kh9pc.ipynb)

### KH-9 Mapping Camera Images

The preprocessing method follows the method of [Dehecq et al. (2020)](https://www.frontiersin.org/journals/earth-science/articles/10.3389/feart.2020.566802/full). In brief:

1. the position of the reseau markers (dark crosses) is detected at subpixel resolution using a convolution with a cross-like kernel.
2. outliers are rejected by removing markers that are not regularly spaced (1 cm)
3. a degree 3 polynomial transformation is first applied to remove rotation and scaling, then a Thin Plate Spline interpolation is used to calculate the distortion at each pixel.
4. the two pieces are stitched together by matching the areas of overlap using NCC.
5. the image is cropped to a fix distance from the outermost markers to keep a constant image dimension among the images
6. the reseau markers are filled with inpainting.

## Installation

Clone the repository and create the conda environment:

```bash
git clone https://github.com/IGE-OpenReproLab2025/godinlu-2025-history.git
cd godinlu-2025-history
conda env create -f environment.yml
```

Some of the raw images are retrieved using [`usgsxplore`](https://github.com/adehecq/usgs_explorer/), which requires [user credentials](https://github.com/adehecq/usgs_explorer?tab=readme-ov-file#credentials).

```bash
export USGS_USERNAME=<your_username>
export USGS_TOKEN=<your_token> 
```

Once your environment is correctly setup, launch Jupyter and select the history kernel to run a notebooks. For OpenReproLab run only this notebook :  [casa_grande_aerial.ipynb](notebooks/casa_grande_aerial.ipynb)

## License

`history` is distributed under the terms of the [Apache-2.0](https://spdx.org/licenses/Apache-2.0.html) license.

The pre-processed images dataset is distributed under the terms of the [Creative Commons Zero v1.0 Universal license (CC0 1.0)](https://creativecommons.org/publicdomain/zero/1.0/), allowing unrestricted use, distribution, and reproduction.
