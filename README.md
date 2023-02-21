# A large-scale fMRI dataset for object recognition in naturalistic scenes
Natural Object Dataset (NOD), a large-scale fMRI dataset containing responses to 57,620 naturalistic images from 30 participants. NOD strives for a balance between sampling variation between individuals and sampling variation between stimuli. The dataset contains raw data, derived data from fMRIPrep and Ciftify, and suface-based analyzed data.   
To get more details, please refer to the paper at {website} and the dataset at https://openneuro.org/datasets/ds004496/

## Preprocess procedure
The MRI data were preprocessed by fMRIprep 20.2.142. Detailed information on fMRIprep pipelines can be found in the online documentation of the fMRIPrep(https://fmriprep.org). Then, all the preprocessed individual fMRI data were registered onto the 32k fsLR space using the Ciftify toolbox.

**code: ./volume_preprocess/preprocess_for_subjects.sh**

Detailed usage notes are available in codes, please read carefully and modify variables to satisfy your customed environment.

## Surface-based process
A high-pass temporal filter (cutoff = 128s) and a spatial smooth (FWHM = 4mm) were applied to the surface data from each run. 

*ciftify*

**code: ./surface_preprocess/surface_preprocess.sh**
      **./surface_preprocess/ciftify_subject_fmri.py**

*temporal filter*

**code: ./surface_preprocess/higpass_cifti.py**

*spatial smooth*

**code: ./surface_preprocess/spatial_smooth_cifti.py**

In most circumstances the only necessary operation it to change the path to dataset, other optional settings are explained by annotations.

## Validation
Some neccessary auxilary files have been stored in the *supportfiles* folder.
For computational convenience, some intermediate files will be generated by codes.
### Reliability
Customed GLM analyses were conducted on the surface data to deconvovle the hemodynamic effects of BOLD signal.

**code: ./validation/GLM.py**

The reliability of the BOLD response evoked by naturalistic stimuli was evaluated for both the ImageNet and COCO experiments. For the ImageNet experiment, we measure the test-retest reliability of the ImageNet experiment by computing the Pearson correlation between the BOLD responses of the 1000 categories (i.e., beta series) from each pair of sessions within each of the nine participants. 

**code: ./validation/reliability_imagenet.py**

For the COCO experiment, we assessed the test-retest reliability of responses to these images by calculating the Pearson correlation between the beta series of 120 images from the odd and even runs on each vertex.

**code: ./validation/reliability_code.py**

### Animacy brain map
We next evaluated whether our fMRI data from a large set of naturalistic stimuli could consistently reveal the animacy map across individuals. Specifically, we contrasted animate versus inanimate categories from the ImageNet experiment within each of the thirty participants, with a total of 410 animate and 590 inanimate categories. The statistic maps for the contrast between animate and inanimate categories were computed with a t-test on each participant.

**code: ./validation/animacymap.py**

### Functional localizer
#### retinotopic mapping
The fMRI data from the retinotopic mapping experiment were analyzed by a population receptive field (pRF) model implemented in the analyzePRF toolbox (http://cvnlab.net/analyzePRF/) to characterize individual retinotopic representation. Make sure to download required software mentioned in the code.

**code: ./validation/pRF_analysis.m**

#### category-selective localizer
A GLM analysis was conducted for the category-selective localizer experiment to define subject-specific category-selective regions. A boxcar kernel convolved with a gamma hemodynamic response function and its temporal derivative was used to model BOLD signal changes for each of the ten categories of stimulus in each run.

**code: ./validation/category-selective_localizer.py**

### encoding model
We combined the data from ImageNet, COCO, and functional localizer experiments to build an encoding model to replicate the hierarchical correspondences of representation between the brain and the DCNN. The encoding models were built to map artificial representations from each layer of the pre-trained AlexNet to neural representations from each area of the ventral visual pathway as defined in the multimodal parcellation atlas. Artificial representations from DCNN were abtained by DNNBrain toolbox (https://dnnbrain.readthedocs.io/en/latest/index.html), some parts of the code are dependent the toolbox, please make sure the toolbox is set on your machine before running the encoding model code.

**code: ./validation/DNN-based_prf-encoding-model.py**




