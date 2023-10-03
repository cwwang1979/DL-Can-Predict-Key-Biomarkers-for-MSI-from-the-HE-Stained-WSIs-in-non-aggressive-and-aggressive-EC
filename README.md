# Deep Learning Can Predict Key Biomarkers for Microsatellite Instability Directly from the H&E-Stained Whole Slide Images in non-aggressive and aggressive Endometrial Cancer
## Setup

#### Requirerements
- ubuntu 18.04
- RAM >= 128 GB
- GPU Memory >= 24 GB
- GPU driver version >= 418.152
- CUDA version >= 10.1
- cuDNN version >= 7.5.1

#### Download
Execution file, configuration file, and models are download from the [zip](https://drive.google.com/file/d/1T2tQGGSeKSjfLlqSxyoVPjQXvJ3kySg-/view?usp=sharing) file.  (For reviewers, "..._cwlab" is the password to decompress the file.)

#### File structure
```
TCGA_WSI_Ovary_Inv3/
│
├── Data/ - training and testing data location
│   ├── BB_tileout/
│   │   ├── TCGA-04-1335-01A-01-BS1.svs/
│   │   │   ├── 10_2.bmp
│   │   │   ├── 10_3.bmp
│   │   │   ├── 10_4.bmp
│   │   │   │       ⋮
│   │   │   └── 20_11.bmp
│   │   │
│   │   └── TCGA-04-1342-01A-01-BS1.svs/
│   │       ├── 30_3.bmp
│   │       ├── 32_1.bmp
│   │       ├── 33_4.bmp
│   │       │       ⋮
│   │       └── 56_11.bmp          
│   │
│   │
│   └── WSI_Image/
│       ├── TCGA-04-1335-01A-01-BS1.svs
│       ├── TCGA-04-1342-01A-01-BS1.svs
│       ├── TCGA-10-0928-01A-01-BS1.svs
│       │       ⋮
│       └── TCGA-61-1730-11A-01-TS1.svs
│
├── Preprocessing/ - Location for storing preprocessed images
│   ├── TCGA-04-1335-01A-01-BS1.svs/
│   │   └── TCGA-04-1335-01A-01-BS1.bmp
│   │       ⋮
│   └── TCGA-04-1342-01A-01-BS1.svs/
│       └── TCGA-04-1342-01A-01-BS1.bmp        
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
    │   └── Ovary_TCGA_MSI50%_weight_iter_1050000.caffemodel
    ├── deploy.prototxt
    └── inference.py - execution file

```

## Steps

#### 1. Data preparation
Place the Whole slide image in Data/WSI_Image/.

Then in a terminal run:
```
./TileScale
```

After running in a terminal, the result will be produced in folder named 'Data/BB_tileout', like the following structure.
```
BB_tileout/
├── TCGA-04-1335-01A-01-BS1.svs/
│   ├── 10_2.bmp
│   ├── 10_3.bmp
│   ├── 10_4.bmp
│   │       ⋮
│   └── 20_11.bmp
│   
└── TCGA-04-1342-01A-01-BS1.svs/
    ├──30_3.bmp
    ├──32_1.bmp
    ├──33_4.bmp
    │       ⋮
    └── 56_11.bmp     
```


#### 2. List process
make two text files 'train.txt' and 'test.txt' file in the folder '/List/'.

The content structure of the text file is as follows:
```
train.txt
│
├── TCGA-04-1335-01A-01-BS1.svs/38_50.bmp,1
├── TCGA-04-1342-01A-01-BS1.svs/55_20.bmp,0
├── TCGA-10-0928-01A-01-BS1.svs/58_30.bmp,1
│        ⋮
└── TCGA-61-1730-11A-01-TS1.svs/60_10.bmp,0

test.txt
│
├── TCGA-12-1335-01A-01-BS1.svs/38_50.bmp,1
├── TCGA-16-1342-01A-01-BS1.svs/55_20.bmp,0
├── TCGA-30-0928-01A-01-BS1.svs/58_30.bmp,1
│        ⋮
└── TCGA-58-1730-11A-01-TS1.svs/60_10.bmp,0

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
./Patch_selection train.txt
```
After running in a terminal, the .txt results will be produced under the folder '/List' and the filename will be PatchSelection_train.txt.


#### 5. Model Selection
Open the Model_selection.py file to set up the storage location of training models and the location of training list("PatchSelection_train.txt") to use.

Then in a terminal run:
```
python Model_selection.py
```
After running in a terminal, the result will be display on the terminal window, record the model name and Copy it to the folder 'inference/Model'.


#### 6. Data preprocessing for inference
Utilize the 'locate' file to locate the coordinates of noise in the image.

Then in a terminal run:
```
./locate
```
After running in a terminal, the .txt results will be produced under the folder '/List' and the filename will be stain_area.txt. 

Next, use the 'Preprocess' file to remove noise from the tiles.

Then in a terminal run:
```
./Preprocess
```
After running in a terminal, the .txt results will be produced under the folder '/List' and the filename will be Preprocessed.txt. 


#### 7. Patch Selection for inference
Utilize the 'Patch_selection' file to choose the tiles for inference.

Then in a terminal run:
```
./Patch_selection Preprocessed.txt
```
After running in a terminal, the .txt results will be produced under the folder '/List' and the filename will be PatchSelection_test.txt. 


#### 8. Testing
Open the "inference.py" file to set up the storage location of training models and the location of testing list("PatchSelection_test.txt") to use.

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

