# Washover ML
Classifier for detecting washover in post-storm images

### Data
- Post-storm images from NOAA (Downloaded via [`psi-collect`](https://github.com/UNCG-DAISY/psi-collect))
- Training and validation data from Hurricane Florence (data [here](https://doi.org/10.6084/m9.figshare.11604192.v1))
- Testing data from Hurricane Michael

### Code
- Finetuned VGG16 model


### Directory Structure:

Should look like:

```{sh}
Users/ebgoldstein/WashoverML
                    ├── readme.md
                    ├── license
                    ├── data
                    │   ├── raw
                    │   ├── training                  
                    │   │    ├── nowashover
                    │   │    │     ├── C26047788.jpg
                    │   │    │     └── ...
                    │   │    └── washover
                    │   ├── validation
                    │   └── testing
                    └── src
                        ├── FullRetrain_VGG16.ipynb
                        ├── GradCAM.ipynb
                        └── .....
                        
                        
```
