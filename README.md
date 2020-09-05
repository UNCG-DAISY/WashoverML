# Washover ML
Classifier for detecting washover in NOAA [Emergency Response Imagery](https://storms.ngs.noaa.gov/).

![Washover](https://github.com/UNCG-DAISY/WashoverML/blob/master/image.png)

### Project Structure:

 The notebooks in `src` are all included in this repository, but the data and model are excluded via the `.gitignore`. Therefore the user must:
1. download the model weights from link below
2. download the appropriate imagery from the links below 
3. sort the imagery into the correct directories.

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
                    │   └── testing
                    └── src
                        ├── FullRetrain_VGG16.ipynb
                        ├── GradCAM.ipynb
                        ├── saved_VGG16_retrain
                        └── .....                  
```

### Data

This project uses post-storm images from Hurricane Florence (2018) and Hurricane Michael (2018). All images are retreived from NOAA via [`psi-collect`](https://github.com/UNCG-DAISY/psi-collect).

- Training and validation data are from Hurricane Florence. We use two groups of images. First, we use images that were labeled as part of a previous data release [here](https://doi.org/10.6084/m9.figshare.11604192.v1). There is class imbalance in this set of labels - images with washover are the rare class. We therefore add new examples of washover from the same group of images. These additional images are listed in the XXXX.csv in the repo. 
- Testing data is from Hurricane Michael. These images have been labebled by two people into washover/no washover classes. The images and their class are listed in the XXXX.csv in the repo. 

Note that users are responsible for downloading the Hurricane Florence images and Hurricane Michael imagery. Florence imagery should be placed in the `/data/raw` directory, in appropriate folders (`/washover` and `/nowashover`). 

Users must also make directories for training and validation data (i.e., `data/training/washover`,`data/training/nowashover`, `data/validation/washover`,`data/validation/nowashover`). The `PictureSplitter.ipynb` will take images from the raw directory and place them into training and validation directory

The test data - Hurricane Michael imagery - should be placed in the appropriate directories: `/testing_michael/washover` and `/testing_michael/nowashover`

### Code
This repository has 4 notebooks. 
- A routine to split data into testing and validation set `pictureSplitter`
- Code to finetuned the VGG16 model starting from imagenet weights and using 400 x 400 NOAA images `FullRetrain_VGG16`
- Code to test the model with Hurricane Michael imagery `Hurricane_Michael_Test`
- Code to look at GradCAM with Hurricane Michael test imagery `GradCAM`

### Model Weights
- Weights can be downloaded [here](https://drive.google.com/file/d/1zVXNsmNJToVHXXOuIIg7NSkR6J-dn9QR/view?usp=sharing). It should be unzipped and put in `/src/`.