# Deep Learning Can Predict Key Biomarkers for Microsatellite Instability Directly from the H&E-Stained Whole Slide Images in non-aggressive and aggressive Endometrial Cancer
## Setup

#### Requirerements
- ubuntu 18.04
- RAM >= 16 GB
- GPU Memory >= 12 GB
- GPU driver version >= 418.56
- CUDA version >= 10.1
- cuDNN version >= 7.6.4

#### Download
Execution file, configuration file, and models are download from the [zip](https://drive.google.com/file/d/1Zg6NodzJeDkD2y9Vu1ueS3ZePze4VzvK/view?usp=sharing) file.  (For reviewers, "..._cwlab" is the password to decompress the file.)

#### File structure
```
Deep Learning Can Predict Key Biomarkers for Microsatellite Instability Directly from the H&E-Stained Whole Slide Images in non-aggressive and aggressive Endometrial Cancer
Setup/
│
├── Data/ - training and testing data location
│   ├── BB_tileout/
│   │   ├── TCGA-2E-A9G8-01.svs/
│   │   │   ├── 10_2.bmp
│   │   │   ├── 10_3.bmp
│   │   │   ├── 10_4.bmp
│   │   │   │       ⋮
│   │   │   └── 20_11.bmp
│   │   │
│   │   └── TCGA-4E-A92E-01.svs/
│   │       ├── 30_3.bmp
│   │       ├── 32_1.bmp
│   │       ├── 33_4.bmp
│   │       │       ⋮
│   │       └── 56_11.bmp          
│   │
│   │
│   └── WSI_Image/
│       ├── TCGA-2E-A9G8-01.svs
│       ├── TCGA-4E-A92E-01.svs
│       ├── TCGA-A5-A0G3-01.svs
│       │       ⋮
│       └── TCGA-5B-A90C-01.svs
│
├── Preprocessing/ - Location for storing preprocessed images
│   ├── TCGA-2E-A9G8-01.bmp
│   │       ⋮
│   └── TCGA-4E-A92E-01.bmp
│              
│
├── List/ - demo list
│   ├── train.txt
│   └── test.txt
│
├── Training/
│   ├── solver.py - execution file
│   ├── Model_selection.py
│   ├── voc_layers.py
│   ├── voc_layers.pyc
│   ├── Model/ - storage location of training models
│   └── network/
│       ├── solver.prototxt - configuration file
│       ├── train_weight.prototxt
│       └── deploy.prototxt
│
└── Inference/ 
    ├── Model/ - demo model
    │   │── G1G2.caffemodel       
    │   └── G3.caffemodel
    ├── deploy.prototxt
    └── inference.py - execution file

```

## Steps

#### 1. Foreground Patch Selection
Place the Whole slide image in Data/WSI_Image/.

Then in a terminal run:
```
./FPS
```

After running in a terminal, the result will be produced in folder named 'Data/BB_tileout', like the following structure.
```
BB_tileout/
├── TCGA-2E-A9G8-01.svs/
│   ├── 10_2.bmp
│   ├── 10_3.bmp
│   ├── 10_4.bmp
│   │       ⋮
│   └── 20_11.bmp
│   
└── TCGA-4E-A92E-01.svs/
    ├── 30_3.bmp
    ├── 32_1.bmp
    ├── 33_4.bmp
    │       ⋮
    └── 56_11.bmp     
```


#### 2. List process
make two text files 'train.txt' and 'test.txt' file in the folder '/List/'.

The content structure of the text file is as follows:
```
train.txt
│
├── TCGA-2E-A9G8-01.svs/38_50.bmp,1
├── TCGA-4E-A92E-01.svs/55_20.bmp,0
├── TCGA-A5-A0G1-01A-01-TS1.svs/58_30.bmp,1
│        ⋮
└── TCGA-5S-A9Q8-02/60_10.bmp,0

test.txt
│
├── TCGA-A5-A0GV-02.svs/38_50.bmp,1
├── TCGA-A5-A0R7-01.svs/55_20.bmp,0
├── TCGA-A5-A0GR-01.svs/58_30.bmp,1
│        ⋮
└── TCGA-A5-A0GW-01.svs/60_10.bmp,0

```


#### 3. Trainining
Open the "solver.py" and "voc_layers.py" files to set up the storage location of training models and the location of training list("train.txt") to use.

Then in a terminal run:
```
python solver.py
```


#### 4. Patch Selection for training
After the training is completed, use the 'Patch_selection' file to select tiles for model selection.

Then in a terminal run:
```
./IPS train
```
After running in a terminal, the .txt results will be produced under the folder '/List' and the filename will be train_IPS.txt.


#### 5. Model Selection
Open the Model_selection.py file to set up the storage location of training models and the location of training list("train_IPS.txt") to use.

Then in a terminal run:
```
python Model_selection.py
```
After running in a terminal, the result will be display on the terminal window, record the model name and Copy it to the folder 'inference/Model'.


#### 6. Patch Selection for inference
Utilize the 'Patch_selection' file to choose the tiles for inference.

Then in a terminal run:
```
./IPS test
```
After running in a terminal, the .txt results will be produced under the folder '/List' and the filename will be test_IPS.txt. 


#### 7. Testing
Open the "inference.py" file to set up the storage location of training models and the location of testing list("test_IPS.txt") to use.

Then in a terminal run:
```
python inference.py
```


## License
This extension to the Caffe library is released under a creative commons license, which allows for personal and research use only. For a commercial license please contact Prof Ching-Wei Wang. You can view a license summary here:  
http://creativecommons.org/licenses/by-nc/4.0/


## Contact
Prof. Ching-Wei Wang  
  
cweiwang@mail.ntust.edu.tw; cwwang1979@gmail.com  
  
National Taiwan University of Science and Technology

