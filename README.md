# Washover ML
[![DOI](https://zenodo.org/badge/290877412.svg)](https://zenodo.org/badge/latestdoi/290877412)

Classifier for detecting washover in NOAA [Emergency Response Imagery](https://storms.ngs.noaa.gov/).

![Washover](https://github.com/UNCG-DAISY/WashoverML/blob/master/image.png)

## Project Structure:

 The notebooks in `src` are all included in this repository, but the data and model are excluded via the `.gitignore`. Therefore the user must:
1. download the appropriate imagery from the links below 
2. sort the imagery into the correct directories
3. download the model weights from link below

The project directory structure should look like this:

```{sh}
          ../WashoverML
                    ├── readme.md
                    ├── license
                    ├── data
                    │   ├── raw
                    │   │    ├── nowashover
                    │   │    │     ├── C26047788.jpg
                    │   │    │     └── ...
                    │   │    └── washover
                    │   ├── training                  
                    │   ├── validation
                    │   ├── testing_michael
                    │   ├── manifest
                    │   │    ├── Florence_nowashover.txt
                    │   │    └── ...
                    │   └── VGG_Michael_results.csv
                    └── src
                        ├── FullRetrain_VGG16.ipynb
                        ├── GradCAM.ipynb
                        ├── saved_VGG16_retrain
                        └── .....                  
```

## Imagery

This project uses post-storm images from Hurricane Florence (2018) and Hurricane Michael (2018). 

- Training and validation data are from Hurricane Florence. We use two groups of images. First, we use images that were labeled using the [Coastal Image Labeler](https://github.com/UNCG-DAISY/Coastal-Image-Labeler) and subject to a previous data release [here](https://doi.org/10.6084/m9.figshare.11604192.v1). There is class imbalance in this set of labels - images with washover are the rare class. We therefore add new examples of washover from the same group of images. These list of all Hurricane Florence images used in the project are in `data/manifest/`, specifically `Florence_nowashover.txt` and `Florence_washover.txt`. 
- Testing data is from Hurricane Michael. These images have been labebled by two people into washover/no washover classes. These list of all Hurricane Michael images used in the project are in `data/manifest/`, specifically `Michael_nowashover.txt` and `Michael_washover.txt` in the repo.  

### Retrieving Imagery

Users are responsible for downloading the publicly avaiable Hurricane Florence and Hurricane Michael imagery. This can be done by downloading the catalogs using the NOAA [Emergency Response Imagery](https://storms.ngs.noaa.gov/) website OR using [`psi-collect`](https://github.com/UNCG-DAISY/psi-collect). The needed archives are:

Hurricane Florence from Sept. 17th 2018 (A) 
Hurricane Michael from Oct. 11th 2018 (A)

The `psi-collect` commands to download the NOAA image archives are:

```{sh}
pstorm collect -s Florence 20180917a_jpgs -d
pstorm collect -s Michael 20181011a_jpgs -d
```

Note that the archives contain many more images than used in this study, and specific images need to be moved to proejct directories.

### Sorting imagery in appropriate directories

The Hurricane Florence imagery listed in the `data/manifest` text files should be placed in `/data/raw` directory, in appropriate folders (`/washover` and `/nowashover`). Users must also make directories for training and validation data (i.e., `data/training/wash`,`data/training/nowash`, `data/validation/wash`,`data/validation/nowash`). The `PictureSplitter.ipynb` code will take images from the raw directory and place them into these training and validation directories. 

Hurricane Michael imagery listed in the `data/manifest` text files should be placed in the appropriate directories: `/testing_michael/washover` and `/testing_michael/nowashover`.

We do not provide a code to move images into directories, but we accomplished this task by importing the txt files into python as lists, adding the path the image files name string, then iterating through the list to move the images to the designated folders using `shutil.copy` (i.e., Florence images to `raw/washover`and `raw/nowashover`, Michael images to `testing_michael/wash` and `testing_michael/nowash`). For Florence images, the `PictureSplitter.ipynb` code will take images from the raw directory and place them into training and validation directories.  

## Code
This repository has 4 notebooks. 
- A routine to split data into testing and validation set `PictureSplitter.ipynb`
- Code to finetuned the VGG16 model starting from imagenet weights and using 416px x 416px NOAA images `FullRetrain_VGG16.ipynb`
- Code to test the model with Hurricane Michael imagery `Hurricane_Michael_Test.ipynb`
- Code to look at GradCAM with Hurricane Michael test imagery `GradCAM.ipynb`

## Model Weights
- Weights can be downloaded [here](https://drive.google.com/file/d/1zVXNsmNJToVHXXOuIIg7NSkR6J-dn9QR/view?usp=sharing). It should be unzipped and put in `/src/`.
