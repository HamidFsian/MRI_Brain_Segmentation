# MRI_Brain_Segmentation
You will find a jupyter nootbook that perform Brain Segmentation using BRATS 2020 database.
I used a 2D U-NET to performs brain segmentation. 
I could not upload my pre-trained model, but feel free to e-mail me for it.



## Problem definiton
Each pixel on image must be labeled:
 - Pixel is part of a tumor area (1 or 2 or 3) -> can be one of multiple classes / sub-regions
 - Anything else -> pixel is not on a tumor region (0)
 The sub-regions of tumor considered for evaluation are: 1) the "enhancing tumor" (ET), 2) the "tumor core" (TC), and 3) the "whole tumor" (WT) The provided segmentation labels have values of 1 for NCR & NET, 2 for ED, 4 for ET, and 0 for everything else.
 ## Some Idea about the BRATS dataset
The dataset from Brats Challenge contains two folders, train and validation. In the validation folder, there are no ground truths. I believe they are only for the evaluation of the challenge and their internal process of evaluation, but I am not sure about this.

In the train folder there are 369 folders of data (also with ground truths), so I use those for all purposes (train/test/validation). Each of these folders contains 4 sequences of MRI (flair, t1, …) and ground truth (segments -> seg), with file name like /BraTS20_Training_001_seg.nii .

All the data are available in NiFTi format, so you can easily get the volume of data with some nifti library. Since the data are 3D and we use 2D U-NET, we split it as slices. original volume shape is (240,240,155), so there are 155 axial slices. (what means axial - check this image?). Each slice of segment then contains only 4 values (0,1,2,4) where 0 means there is no tumor and the other are different tumor classes. In this notebook we change the values from 4 to 3, so it is easier to use.

## Model Creation 
The u-net is convolutional network architecture for fast and precise segmentation of images. Up to now it has outperformed the prior best method (a sliding-window convolutional network) on the ISBI challenge for segmentation of neuronal structures in electron microscopic stacks. It has won the Grand Challenge for Computer-Automated Detection of Caries in Bitewing Radiography at ISBI 2015, and it has won the Cell Tracking Challenge at ISBI 2015 on the two most challenging transmitted light microscopy categories (Phase contrast and DIC microscopy) by a large margin (See also our annoucement).
